---
title: "New AutoSpotting Bugfix Release - 1.3.1"
date: 2024-04-29T00:00:00+00:00
description: "Critical fixes for Default VPC placement and Lambda timeout issues"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "bugfix", "releases", "leanercloud"]
featured_image: "/images/blog/leanercloud/2024-04-29-bugfix-release-1-3-1.jpg"
draft: false
---

## Fixing regressions in 1.3.0: incompatibility with instances out of the DefaultVPC and Lambda timeout when replacing Spot instances


 April 29, 2024

Hello,

After [releasing](https://leanercloud.beehiiv.com/p/new-autospotting-releases) AutoSpotting 1.3.0 a couple of weeks ago, I've been working with a few customers who noticed a couple of regressions introduced in this latest version:

- **AutoSpotting failed to launched instances except in the Default VPC (major)**.
- **AutoSpotting Lambda timeouts** while waiting for terminated Spot instances to be in Running State in case the Grace Period is longer than 2 minutes **(medium)**, resulting in failure to clear suspended ASG. processes**.**

This release is meant to address those and is **strongly recommended to users that don't use Default VPCs for EC2 instances launched by AutoSpotting**.

If you're curious to learn more details about these regressions, keep reading below:

1. **Regression related to instance placement in the Default VPC -** for a lot of time, when running Spot instances, we used to place instances in the right Availability Zone and VPC by using the Subnet ID.

   This was working very well and is future-proof considering the subnet IDs allow distinguishing between VPCs.

   But some customers are still using the EC2 Classic configuration style in Default VPCs. In such configurations security groups are given by name, and passing the Subnet ID in the same way causes instance launch failures.

   In preparation for 1.3.0 I was working extensively with a customer running such a configuration, who migrated from a 5 years old Open Source AutoSpotting version. They noticed those instance launch failures and while working with them, among a bunch of other fixes included in 1.3.0, I also converted the code to use the Availability Zone names instead of Subnet IDs.  
     
   But at the time I didn't realize that this change would place all AutoSpotting instances in the Default VPC for everyone else.

   In my main test environment I actually use a Default VPC, so everything worked well, but some other customers who updated to 1.3.0 noticed their instances were failing to launch and traced them to be outside of the Default VPC.

   I immediately gave them a hotfix that reverted that change, and then for the last week worked with that EC2 Classic customer to figure out a way to run AutoSpotting in a way that supports them as well.

   Starting from version 1.3.1, AutoSpotting will revert to using Subnet IDs for most situations, but in case the configuration is in ECS Classic style, it will still use Availability Zone names that cause instances to launch in the Default VPC.
2. **Regression causing Lambda timeouts when replacing terminating Spot Instances** - The same EC2 Classic customer also uses a configuration with a single instance type using the `current` keyword in the Allowed Instance Types configuration or Autoscaling group's tag override.

   This is a risky configuration, and not something I'd normally recommend, because it forbids any Spot diversification and causes increased Spot instance churn and quite frequent failover to OnDemand, which again is not diversified and could be prone to ICE events.

   They seem to insist on just a single instance type, accepting these consequences, and would rather rely on AutoSpotting's failover to OnDemand instances as a way to ensure reliability.

   This configuration was a great opportunity to heavily test our failover to OnDemand logic, which normally sees relatively little use in the normal situations where Spot diversification usually allows us to get replacement Spot capacity.

   And in their tests we noticed Lambda was timing out when replacing instances.

   Turns out that for whatever historical reasons I no longer remember, we were waiting for Spot instances about to be terminated to reach the Running and In Service state in the AutoScaling group.

   This wasn't previously a problem, since the Spot instances were always running when we terminated them.

   But in 1.3.0 we also changed the replacement logic to wait for newly launched instances to enter the Running and In Service state, plus for the group's grace period, before terminating the replaced instances.

   In my test environment I used a relatively short grace period which allowed me to be faster at testing, and that didn't uncover this issue.

   But the customer had a longer grace period, longer than the 2 minute Spot termination notice time, so the Spot instances were terminated by the time we triggered the waiting logic, which would never find the Spot instances in Running state and timing out the Lambda runs, which then left the ASG with some suspended processes until the next AutoSpotting instance replacement could clear them.

   It also didn't help that the logs weren't really saying anything, all we saw were Lambda timeouts after 15 minutes.

   Unlike the Default VPC regression, this was relatively harmless, except for the small Lambda costs in case your Lambda use is above the Lambda free tier and the suspended ASG processes which kept the ASG at constant capacity.

I'm happy we found and fixed these relatively quickly, and going forward I'm working on improving my test setup to also cover these situations.

## How to update

As usual, this version is available on the [AWS Marketplace,](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-bugfix-release-1-3-1) make sure to use `1.3.1-0` or newer in the Docker image tag configuration.

That's it for now,

-Cristian

#### Keep reading

[![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/new-autospotting-bugfix-release-131/image-2.png)

## Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info


## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


## Current OpenTofu contributors vs. pledged FTEs

OpenTofu Github stats half a year after the fork.


View more
