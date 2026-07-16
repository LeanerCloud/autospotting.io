---
title: "How we maximized task density on our ECS cluster by avoiding burstable instances"
seo_title: "Maximizing ECS Task Density Without Burstables"
date: 2023-06-28T00:00:00+00:00
description: "Overcoming ECS task density limitations caused by ENI exhaustion on burstable instances"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "ECS", "Optimization"]
tags: ["aws", "ecs", "ec2", "networking", "leanercloud", "task-density"]
featured_image: "/images/blog/leanercloud/2023-06-28-maximized-task-density-ecs.png"
draft: false
---

## And why we had to deploy NAT Gateways in our public subnets


 June 28, 2023

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-2.png)

As I've mentioned in my previous posts, I've been recently working with an AI startup to help them optimize their AWS costs.

After quickly doing the basic things like converting EBS volumes to GP3 using [EBSOptimizer](https://leanercloud.com/ebs-optimizer?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances), right-sized and converted their RDS databases to Graviton and a few other low hanging fruits, the bulk of the work was about converting their individual instances running Docker-compose to ECS.

I elaborated on this widely on my previous [blog](https://leanercloud.beehiiv.com/p/recommended-ecs-instead-kubernetes-latest-customer), so I won't go into details here, but the initial goal was to make their application suitable for Spot instances and adopt Spot in a more reliable manner using [AutoSpotting](https://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances).

We're not there yet, but we're very close to running everything on ECS, so we can next start a rightsizing exercise and qualifying services suitable for Spot instances based on the metrics offered by ECS.

As part of this effort we've also been trying to choose a more cost effective instance type and eventually settled for t3a.2xlarge for our ECS hosts, because we have very little CPU utilization, ideal for such burstable instance types.

## Task density challenges

Soon after the team noticed something interesting: even though the t3a.2xlarge instance we were using had almost half the vCPUs and more than half the memory available (out of a total of 8 vCPUs and 32 GB of memory), it could only run 3 ECS tasks:

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-3.png)

This t3a.2xlarge instance won't get any new tasks scheduled on it.

## What's going on here?

As mentioned in the previous blog, we're using the `awsvpc` networking mode, which is great for allocating dedicated IPs to tasks, avoiding port conflicts and the use of dynamic port ranges and allowing us to configure security groups for the tasks.

This is important for us because the applications all listen on the same monitoring port, and if we used the `bridge` mode with dynamic ports the monitoring system wouldn't be aware of the dynamic ports and couldn't connect to them

The way `awsvpc` works is each instance type allocates an ENI(Elastic Network Interface) for each ECS task.

The problem is each instance type has a fixed number of ENIs, depending on the size of the instance, and the scheduler will consider the instance busy when the ENIs are all exhausted.

I knew about this ENI limitation since a long time, but the development team wasn't aware of it, as it's relatively easy to oversee in the ECS docs if you don't pay attention, and also even if you read the docs there aren't clear guidelines on how to overcome it unless you combine the information from a number of documentation pages.

## And a little detour on NAT Gateway

BTW, this is not the only largely unknown drawback of `awsvpc`, we also recently learned it also requires NAT Gateways even though the ECS hosts are in a public subnet (!!!) or otherwise the tasks have no internet access.

As per the [docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking-awsvpc.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances):

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-4.png)

But back to our task density challenges…

## How to get higher task density with ECS?

One way out of this situation would be to switch the networking mode to `bridge` and use dynamic ports, but then we lose all the nice things about `awsvpc` I mentioned above, which is something we didn't want.

In order to increase task density while keeping `awsvpc` we can enable ENI trunking, as per the same [docs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking-awsvpc.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances):

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-5.png)

To enable ENI trunking all you need to do is run this AWS CLI command, then all newly launched instances will have it enabled, and ECS should automatically provision more tasks on them:

```
aws ecs put-account-setting-default \
  --name awsvpcTrunking \
  --value enabled
```

This AWS [blog](https://aws.amazon.com/blogs/compute/optimizing-amazon-ecs-task-density-using-awsvpc-network-mode/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances) is also a great resource for enabling ENI trunking, as always much easier to digest than the docs.

## But then nothing happened…

When the team first enabled VPC Trunking and started a few new instances, it still had no effect.

We then realized that the burstable instance types such as our t3a.2xlarge don't support ENI trunking at all. Bummer!

(The full list of instance types that support ENI trunking is available at [here](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/container-instance-eni.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances#eni-trunking-supported-instance-types), also showing the number of tasks with ENI trunking enabled.)

So we ended up converting our configuration from t3a.2xlarge to m6a.2xlarge, which has the same CPU and memory size but supports a much higher task density when ENI trunking is enabled:

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-6.png)

Yes, m6a.2xlarge is a bit more expensive than t3a.2xlarge ($252.28 vs $219.58 monthly in Virginia), but as soon as we started using it, the ECS scheduler filled it up with tasks, as expected, so we need to run much less of them to run the same tasks:

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-7.png)

m6a.2xlarge can run way more than 3 tasks with ENI trunking enabled

## Final words

Even though I knew about the ENI limit and that VPC Trunking is a great workaround, I wasn't aware the burstable instance types don't support it, and also wasn't aware of the NAT Gateway requirement for public subnets.

I hope that you learned something new from this blog so you don't have to learn these the hard way.

If you liked this blog and want to comment about it, you can join the conversations on [reddit](https://www.reddit.com/r/aws/comments/14l8os2/how_we_maximized_task_density_on_our_ecs_cluster/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances) or [HackerNews](https://news.ycombinator.com/item?id=36505685&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances). To see more of these you can also subscribe or see previous previous posts [here](https://leanercloud.beehiiv.com/).

You can also check out my **[YouTube channel](https://www.youtube.com/@Leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances)****,** [**podcast**](https://anchor.fm/leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances), follow me on [Twitter](https://twitter.com/magheru_san?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances) or connect with me on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances) for more of my content.

And If you're interested to optimize your AWS environment for costs, performance or anything in between with a deeply technical ex-AWS Specialist SA, I'm happy to[help](https://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-we-maximized-task-density-on-our-ecs-cluster-by-avoiding-burstable-instances).

#### Keep reading

[![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-8.png)

## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


## New AutoSpotting bugfix release - 1.3.1

Fixing regressions in 1.3.0: incompatibility with instances out of the DefaultVPC and Lambda timeout when replacing Spot instances


View more
