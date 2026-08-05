---
robots: "index, follow"
title: "Another New AutoSpotting Release in Less Than a Week"
seo_title: "A New AutoSpotting Release in Under a Week"
date: 2023-04-15T00:00:00+00:00
description: "AutoSpotting 1.2.1-0 addresses critical ECS load balancer draining issues during Spot terminations"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "ecs", "spot-instances", "load-balancer", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.jpg"
draft: false
---

## Fixing ECS load balancer draining


 April 15, 2023

Over the last 7 years I've been working hard to make AutoSpotting the best solution for running Spot Instances in Autoscaling groups.  
  
Just a few days ago I released our [biggest release ever](https://leanercloud.beehiiv.com/p/leanercloud-product-updates) which makes AutoSpotting managed groups of Spot instances even more resilient to low capacity situations than the vast majority of Autoscaling groups configured with a single OnDemand instance type.  
  
As I always keep improving the experience based on user feedback, I just released a new version of AutoSpotting that improves reliability for ECS users, making AutoSpotting with ECS a more highly available solution than plain ECS itself!  
  
A few weeks back an AutoSpotting user reported an interesting issue with their ECS setup: each Spot termination in their ECS cluster with Spot capacity managed by AutoSpotting resulted in dropped connections and 5xx errors returned to users.  
  
After lots of investigations, today I was finally able to reproduce and fix the issue.  
  
For a little background, AutoSpotting never handled Spot instance load balancer draining itself but relied on the AutoScaling group or ECS to drain the connections from the load balancers when Spot instances are terminated.  
  
While Autoscaling worked pretty well, it turns out that ECS is very slow at triggering the draining, sometimes starting the load balancer draining just a few seconds before (or sometimes even after!) the Spot instance has been shut down:

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-2.png)

This leads to dropped connections and users getting 5xx errors when Spot instances are terminated.  
  
To make it clear, this is not only affecting AutoSpotting users, but anyone using ECS with Spot instances, so if you use ECS with Spot instances I'd recommend you to have a look into this.  
  
So earlier today I implemented earlier deregistration, available in the next release of AutoSpotting.  
  
The deregistration API calls are done by AutoSpotting in less than 10 seconds after the Spot termination event was fired, which should give plenty of time for the connections to be drained cleanly from ECS tasks without users getting errors.

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-3.png)

And as we now also immediately launch the replacement Spot instances with diversified failover to OnDemand instances, we give even more time for the new instances to start running the application and we reduce the time of running at reduced capacity as much as possible.  
  
After the latest AutoSpotting release we had just a few days ago, I was thinking to take a few more weeks until the next release, but this is such a big issue that I decided to change my plans, and released it immediately.  
  
Check out the latest available version of AutoSpotting, 1.2.1-0 already available on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=another-new-autospotting-release-in-less-than-a-week).  
  
If you run the current release you can install it using this CloudFormation template, just make sure you also use the SourceImageTag "stable-1.2.1-0"  
  
[https://s3.amazonaws.com/autospotting-builds/stable-1.2.1-0/template.yaml](https://s3.amazonaws.com/autospotting-builds/stable-1.2.1-0/template.yaml?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=another-new-autospotting-release-in-less-than-a-week)  
  
And as always, stay tuned, there's more stuff coming soon in the next release.

-Cristian

**Later Edit:**

We noticed that the version `1.2.1-0` only considered the listener port (such as 80) and failed to drain traffic if the instance was listening on other ports.

We just released the version `1.2.1-2`, which correctly handles instances with different ports than the load balancer listener, such as when instances listen on a variety of dynamic ports.

Here's how it looks like in action:

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-4.png)

There's also a small non-functional fix related to the deletion of SQS messages after being processed, which used to be shown in the logs like this:

SCE:2023-04-18T12:10:00 2023/04/18 12:11:09 region.go:522: us-east-1 **Error** deleting on-demand instance i-09e980848cc071f5a launch event message from the SQS Queue https://sqs.us-east-1.amazonaws.com/xxxxx/AutoSpotting.fifo: MissingParameter: The request must contain the parameter ReceiptHandle.

The current version instead shows this message:

SQS:i-0e2ffa0829eb9167c 2023/04/18 10:28:05 region.go:527: us-east-1 Successfully deleted spot instance i-0e2ffa0829eb9167c launch event message from the SQS Queue https://sqs.us-east-1.amazonaws.com/xxxxx/AutoSpotting.fifo

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-5.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Vantage just updated ec2instances.info and released all their code, now what?


## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


View more
