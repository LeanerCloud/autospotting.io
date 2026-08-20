---
robots: "index, follow"
title: "New AutoSpotting Release: Mixed Autoscaling Groups Support"
seo_title: "Mixed AutoScaling Groups Support Released"
date: 2023-07-04T00:00:00+00:00
description: "AutoSpotting now supports AWS AutoScaling groups with Mixed Instances Policy"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "AWS", "Releases"]
tags: ["autospotting", "aws", "autoscaling", "spot-instances", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-07-04-mixed-autoscaling-groups.png"
featured_image_width: 1292
featured_image_height: 105
draft: false
---

## How ECS capacity providers inspired me to fix an ancient AutoSpotting issue


 July 04, 2023

I'm excited to announce that a new version of AutoSpotting is now available.

For some context if you're not familiar with AutoSpotting, it's a tool that makes it easy to adopt Spot instances in existing Autoscaling groups, without requiring configuration changes, by replacing their instances with Spot clones using attach/detach API calls.

For more information about AutoSpotting you can have a look at [AutoSpotting.io](https://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups) or check out our Open Source code on [GitHub](https://github.com/LeanerCloud/AutoSpotting?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups).

## What's new?

This release adds supports for AutoScaling groups with [Mixed Instances Policy](https://docs.aws.amazon.com/autoscaling/ec2/userguide/ec2-auto-scaling-mixed-instances-groups.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups) configurations

These configurations were previously not supported by AutoSpotting in order to avoid race conditions with the Autoscaling groups, and AutoSpotting was silently ignoring them, which confused many new users.

Going forward AutoSpotting with take over such groups much like it does for all other supported AutoSpotting configurations, without the need for any configuration changes and in a way that avoids Infrastructure Code configuration drift. Avoiding configuration drift is critical in this situation because it can cause sudden undesired instance terminations, as you'll see below.

There is also another important bugfix for a regression introduced in the previous version(also impacting new AutoSpotting users) which manifested through the failure to convert existing instances to Spot, only reacting to new instance launches. So this version will also now again gradually convert existing instances to Spot.

## Why does this matter?

These groups first [became available back in 2018](https://aws.amazon.com/blogs/aws/new-ec2-auto-scaling-groups-with-multiple-instance-types-purchase-options/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups) and are very common nowadays, including for configurations like EKS node groups. Many AutoSpotting users experienced issues with the previous lack of support for these configurations, and this was the main impediment to adoption of AutoSpotting by new users lately.

AutoSpotting simply ignored these groups until they were reconfigured to not use Mixed Instances Policy configurations.

Even though the change to make it work was setting a simple checkbox per group in the AWS console and documented in the AutoSpotting FAQ, this was introducing friction to new users.

It was also breaking the AutoSpotting promise of requiring no configuration changes, and many new users had to look at the logs to see what's going on, which was a poor user experience.

## How does it work?

The Mixed Instances Policy of Autoscaling groups can be used to configure a mix of OnDemand/Spot ratio and an optional baseline of OnDemand capacity.

This can even be 100% OnDemand, as you can see below:

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-2.png)

Many users may want to convert those groups to Spot using AutoSpotting without touching their configuration, much like we do for other groups.

The benefit is the fact that you can revert back to OnDemand with minimal effort in case things go wrong, and you get automated diversification without having to move a finger.

And while for configurations that use Spot instances in the mix, Autoscaling groups don't compensate for unavailable Spot capacity, which can cause outages when available capacity is low, AutoSpotting offers failover to OnDemand instances and automated instance type prioritization for the best possible price/performance by automatically prioritizing recent instance types when available.

The problem is whenever a group is configured with a Mixed Instances Policy, it may stop existing instances and start new ones to enforce the configured Spot/OnDemand ratio.

This previously interfered with Spot instances launched by AutoSpotting and attached to the groups, which were immediately terminated and replaced with OnDemand to maintain a 100% OnDemand group, and that's the reason why AutoSpotting ignored those groups.

For a lot of time I was thinking about how to best solve this issue, and only recently figured it out. As I mentioned in the previous posts, I've been working with ECS lately for one of my cost optimization customers, and got some inspiration from its [capacity providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), which use Scale In protection to protect ECS instances from being terminated while they still run ECS tasks.

Similarly, in order to avoid these terminations, I implemented in AutoSpotting support for setting ScaleIn protection for all Spot instances it launches in mixed groups. AutoSpotting maintains ScaleIn protection throughout the lifecycle of the Spot instances, similar to the way ECS Capacity providers maintain ECS capacity in AutoScaling groups.

You can see below how this looks like on a new AutoScaling group managed by the latest version of AutoSpotting:

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-3.png)

Unfortunately that wasn't all, because the Autoscaling group also attempts to launch replacement OnDemand instances that would be used to replace these Spot instances, which it then never terminates itself regardless of the group's desired capacity.

So in order to avoid increasing capacity in the group, we also need to terminate any newly launched On Demand instances that Autoscaling starts when attempting to replace our new Spot instance.

Here's how the process looks like in practice on a fresh group:

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-4.png)

- we first launched an OnDemand instance in the group to have some capacity to work with (the fourth instance).
- AutoSpoting launched a Spot instance replacement,(the second instance) and immediately attached it to the group and protected it from Scale In.
- Autoscaling detected a conflict with the mix configuration, and attempted to stop our new Spot instance but failed because it is protected from Scale In.
- Autoscaling also launched two new OnDemand instances, that would have been used to replace our Spot instance if we allowed that to happen, but often it's a single instance.
- AutoSpotting terminated these new OnDemand instances immediately because they were over the Desired capacity of the group.
- The group eventually accepted our protected Spot instance and no longer attempts to launch OnDemand instance replacements for it.

## Caveats

The use of the ScaleIn protection means our Spot instances won't be terminated by Autoscaling when the group capacity is decreasing, until AutoSpotting clears the ScaleIn protection for them.

We currently do this in every periodic run of AutoSpotting, by default every 30 minutes, looking if the desired capacity decreased and unprotecting a number of Spot instances that the group would try to terminate when scaling in, being careful not to cause rebalancing of capacity across availability zones

So going forward you may occasionally see such protected Spot instances running for some time after the group has scaled in even though the group's desired capacity was since decreased by Autoscaling.

In a future version of AutoSpotting we may make AutoSpotting react faster to scale in events, although I don't think it's a good idea to be too aggressive with this, as it can potentially interfere with other automation that relies on ScaleIn protection, such as the ECS Capacity Providers.

In the meantime you can increase the frequency of our periodic runs to reduce the time of these Spot instances running after decreasing the capacity of the groups.

## How to update?

You can use the latest version of our CloudFormation [template](https://s3.amazonaws.com/autospotting-builds/stable-1.2.2-0/template.yaml?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), or our updated Terraform [module](https://registry.terraform.io/modules/AutoSpotting/autospotting?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), just make sure you update the release version to `stable-1.2.2-0`.

For CloudFormation you can just run the below command:

```
DOCKER_IMAGE_VERSION=1.2.2-0 aws cloudformation \
  update-stack --stack-name=AutoSpotting \
  --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
  --template-url="https://s3.amazonaws.com/autospotting-builds/stable-${DOCKER_IMAGE_VERSION}/template.yaml" \
  --parameters ParameterKey=SourceImageTag,ParameterValue=${DOCKER_IMAGE_VERSION}
```

We tested this thoroughly over the last few days so there should not be any major bugs, but please let us know if you notice anything.

Best regards,  
Cristian  
  
PS: If you liked this blog and want to comment about it, you can join the conversations on [reddit](https://www.reddit.com/r/aws/comments/14qihjn/how_ecs_capacity_providers_inspired_me_to_fix_an/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups) or [HackerNews](https://news.ycombinator.com/item?id=36588095&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups). To see more of these you can also subscribe or see previous previous posts [here](https://leanercloud.beehiiv.com/).

For more of my content you can also check out my [YouTube channel](https://www.youtube.com/@Leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), [podcast](https://anchor.fm/leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), [website](https://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups), follow me on [Twitter](https://twitter.com/magheru_san?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups) or connect with me on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-release-adding-support-for-mixed-autoscaling-groups).

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-5.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Why I just declined a job offer from a billionaire friend


## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


View more
