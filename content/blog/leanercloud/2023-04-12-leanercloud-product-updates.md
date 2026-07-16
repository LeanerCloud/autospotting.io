---
title: "LeanerCloud Product Updates"
date: 2023-04-12T00:00:00+00:00
description: "AutoSpotting 1.2.0-2 delivers the most substantial release in 7 years with major performance and reliability enhancements"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "spot-instances", "updates", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

*After less than 7 weeks from our previous AutoSpotting release, we're happy to announce a new one, our biggest release so far in the entire 7 years history of AutoSpotting.*

*It brings over a dozen improvements in performance, reliability, correctness and ease of use, including the following major enhancements:*

1. ***Proactive Spot instance launch on Spot terminations****, reducing the instance replacement time by avoiding the use of temporary On-Demand instances.*
2. ***Diversified failover to On-Demand instances*** *during periods of high-demand of EC2 capacity provides even more resilience to Insufficient Capacity Errors(ICE) than the typical AutoScaling groups configured with a single instance type.*
3. ***Automatically determine the Spot product for each AutoScaling group****, resulting in a simpler setup, correct instance type selection, accurate billing and higher diversification for Windows, RHEL and Suse Linux Spot instances.*
4. ***Supporting installation of AutoSpotting in any AWS region*** *as its main region, reducing the complexity and networking costs of the previous cross-region setup.*
5. ***Fixed a number of issues related to Security group configuration****, which sometimes caused Spot instances to fail to launch or start with incorrect Security Groups.*

*This updated version of AutoSpotting is available on the* *[AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates)* *and can be installed as usual using CloudFormation or Terraform.*

*You can read below a more detailed explanation of all the new improvements and how they benefit you, and a few other progress updates.*

Hi there,

It's been a few weeks since my last progress update.

It was a bit intense for the last few weeks with tons of things to do, so I didn't have time to write these progress reports weekly like I used to do for the last 6 months.

I then took some days off around Easter, in which I traveled, recharged my batteries and also reflected on what I've been doing, including on how to make these reports more useful for my audience.

To be honest talking about what I've been doing each week was a good way for me to track my progress and also keep myself motivated to do stuff and getting the most out of my time so by the end of the week I have something to talk about.

But it was very time and effort intensive and didn't seem like my audience got much value out of it as it was.

So going forward I'll keep my weekly progress as brief private notes in a personal journal, and instead of focusing on how I spent my time, I'll repurpose these updates to sharing relevant product development news that benefit my users when I have something meaningful to share, and as you can see, I also changed the format a bit.

It will not necessarily be based on a weekly or monthly cadence, although it could still be at a high frequency if I have things to share.

My first such update is about the latest release of AutoSpotting, which I just published on the AWS Marketplace.

## 7 years anniversary AutoSpotting release

This is easily the largest release over the entire history of the project, at least when it comes to the amount of changes. It is packed with reliability, performance and correctness fixes and I'm very excited about it.

By the way, next week it's the 7th year anniversary of my first [blog post](https://mcristi.wordpress.com/2016/04/21/my-approach-at-making-aws-ec2-affordable-automatic-replacement-of-autoscaling-nodes-with-equivalent-spot-instances/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates) in which I announced AutoSpotting to the public, and I'm blown away by the progress we made since.

You can see the brief changelog of this release documented on [GitHub](https://github.com/LeanerCloud/AutoSpotting/discussions/489?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates), and below I'll cover in detail all the changes from this release, with some context on why they matter and how they benefit you as users of AutoSpotting:

### Proactive instance launch on Spot terminations with diversified failover to OnDemand (performance/reliability/cost)

AutoSpotting now reacts to Spot termination events by attempting to launch a new Spot instance directly, saving about a minute of running at reduced capacity when Spot terminations happen.

Previously the group would run for a while at decreased capacity, eventually take notice and spin up an OnDemand instance and then we'd replace that instance with a new Spot instance.

This was slow and suboptimal, so that's why we decided to improve it by intercepting the Instance termination events and launching Spot instances immediately.

Here you can see it in action:

![](/images/blog/leanercloud-content/leanercloud-product-updates/image-2.png)

1. OnDemand instance ending on 587 was started, as this was a new group
2. AutoSpotting replaced it with instance ending in 096, as usual.
3. Spot instance ending in 096 was terminated using Fault Injection Simulator(FIS), just like AWS Spot would terminate it when that capacity would be needed by another customer.
4. AutoSpotting launched a new Spot instance ending in b4c. (new)

We also took control over the failover to on-demand instances when Spot capacity is not available, by attempting to spin up On-Demand instances using the same diversification we have for Spot instead of leaving the ASG to spin up the On Demand instance using the single instance type configured in the launch Configuration or Launch Template.

This makes AutoSpotting managed Autoscaling groups faster to react and to Spot terminations and also more resilient to the so called Insufficient Capacity Errors (ICE) of the single instance type configured in the Launch Template.

The benefits for you are that you get Spot instances launched earlier and also won't run at reduced capacity when Black Friday comes and AWS is sometimes running out of capacity for your single instance type.

AutoSpotting Spot AutoScaling groups should therefore become even more resilient than your plain On-Demand AutoScaling groups with a single instance type configured in the Launch Template or Launch Configuration.

This feature is entirely automated, once you run the latest version you don't need to do anything and it just works.

#### Deep dive into the background and evolution of this feature

Feel free to skip this section, unless you're curious to see how the sausage is made, which some people find interesting 😊

For more than 4 years we had [code](https://github.com/LeanerCloud/AutoSpotting/pull/309?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates) that handled Spot termination events. As a fun fact, this functionality was the inspiration of the current event-driven design of AutoSpotting, which was also built upon the event forwarding infrastructure initially used to build this piece of functionality.

Until recently, AutoSpotting was just terminating the Spot instances proactively as a way to trigger the termination lifecycle hooks, which don't run if the Spot instances are terminated by EC2.

The failover to On-Demand instances was just a nice side effect of keeping the same Launch Configuration/Template configuration, which then would be used to spin up that on demand instance.

But in most cases we replaced this On-Demand instance within seconds, which was suboptimal because besides paying for it, this would also slow down the process of getting the replacement Spot instance, and you'd run for a minute or so at decreased capacity. Also, at times that instance type could also have limited capacity, as the groups remained vulnerable to ICE events.

A few weeks back a customer reported some issues with this functionality, in particular he noticed some load balancer errors when instances were terminated, and when looking deeper into it we noticed how the logic to handle these events had a few flaws and even crashed at times.

When trying to reproduce these issues, I looked at the code implementing this functionality and noticed that it duplicated some other code we have and was doing it in questionable ways so I decided to revamp it and reuse existing primitives.

There was also an old [pull request](https://github.com/LeanerCloud/AutoSpotting/pull/475?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates) draft I started more than a year ago, in which I started working on enhancing this instance termination handling to proactively launch Spot instances when running Spot instances about to be terminated.

So I decided to resume work on that pull request, bringing it up-to-date with the current code base and also used the opportunity to improve the code quality by reusing the existing primitives and avoiding the code duplication.

For a future development the plan is to also implement failover to multiple OnDemand instance types in the event of ICE errors when scaling out, which is currently still not covered.

### Bug fixes on Security group configuration / Reusing Launch templates (correctness)

AutoSpotting used to create temporary launch templates for each instance we launch, by copying the Launch Template or Launch configuration of the group.

This introduced a number of subtle bugs, mostly noticed by customers when it came to security groups: for Launch Templates some customers noticed duplication and running into soft limits that caused instances to stop launching, while for Launch Templates we sometimes failed to pass any Security Groups and getting the Default VPC Security Group as a fallback.

We now reuse Launch Templates configured on the AutoScaling groups and only create them for groups that still use Launch Configurations. We also now reuse Launch Templates across multiple runs, and addressed the Security group issues for both Launch Templates and Launch Configurations.

Besides the fixed bugs, this may be also helpful for people still using Launch Configurations(which have relatively recently been deprecated by AWS) to finally switch to Launch Templates by using the template created by AutoSpotting for their AutoScaling group.

#### Deep dive into the background and evolution of this feature

Also feel free to skip this section, unless you're curious for a little deep dive into this 😊

The current implementation of AutoSpotting uses EC2 Fleets in [Instant](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instant-fleet.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates) mode to spin up Spot instances (and recently also OnDemand, as mentioned above). These allow us to diversify over multiple instance types with a single API call, and also provide the capability to use different capacity allocation strategies.

EC2 Fleets require Launch Templates, and for historic reasons AutoSpotting used to clone the Launch Template and Launch Configuration into a new temporary Launch Template whenever it was launching new Spot instances.

Some people also asked why their instances were showing a different Launch template than the one set on the AutoScaling group, which they found confusing and also the fact that we deleted it made it hard to investigate some issues:

```
“The launch template it refers to lt-xxxxxxxxxx.
Is that generated by AutoSpotting? I don't see it among my launch templates.”
```

The Launch Templates have a pretty complex structure, so it turned out to be quite challenging to correctly copy all their fields.

Some users reported a number of bugs caused by imperfect conversion of the Launch Templates, in particular when it came to the Security Groups.

In some cases the Security Groups were duplicated, so with just 3 security groups, when duplicated we'd be running into the soft limit of 5 security groups, which caused Spot instance to fail launching.

While working on this I also noticed the Launch Configuration Security Groups were copied incorrectly, leading to the use of the Default VPC Security group instead of the security groups configured in the Launch Configurations.

The new version of AutoSpotting will reuse the group's Launch Template to spin up the Spot instances, and only copy the Launch Configurations into Launch Templates. This is because the Launch Template is still needed by the EC2 Fleets, and we want to keep supporting Launch Configurations for as long as people have them, but now we also have a fix for their issue with the Default Security Group.

Besides this issue with Security Groups, this change also avoids an entire class of potential bugs caused by the imperfect clone of Launch Templates when spinning up Spot instances.

### Dropping support for EBS Volume conversion (correctness)

Somewhat related to the Launch Template reuse, mainly because they would only be converted for Launch Configurations, I decided to no longer convert the GP2 EBS volumes to GP3, and IO1 volumes to IO2 at all.

This never actually worked well enough, and is much better covered by my [EBS Optimizer](https://aws.amazon.com/marketplace/pp/prodview-ryzl67mmq3ghk?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates) tool, which is dedicated to such EBS volume conversions.

I recommend anyone who used this feature in AutoSpotting to start using EBS Optimizer for this functionality going forward, since EBS Optimizer is much more robust, flexible and feature reach when it comes to converting EBS volumes to GP3.

The configuration parameters for these EBS volume conversions have been removed from the configuration of the Terraform and Cloudformation infrastructure code.

#### Deep dive the background and evolution of this feature

The idea of GP2 volume conversion to GP3 was first implemented in AutoSpotting soon after the GP3 volumes became available, and then it was soon extended to cover IO1 to IO2.

It seemed a great way to save some additional costs as a bonus of using AutoSpotting, bundling even more value in addition to the Spot savings.

Unfortunately, the implementation was quite complex, mainly because volume configuration can come from a number of places: besides the Block Device mapping configuration in the Launch Templates, there's a similar configuration in the Launch configuration, and also volume type is propagated from the AMI.

AutoSpotting covered some of these, but it wasn't perfect, and also the logic was quite complex and hard to maintain over time.

At some point I thought it would be better to also handle instances that were not launched by AutoSpotting, such as instances for stand-alone instances or instances launched by EMR or Kubernetes Persistent Volume Claims configurations.

So that's how I got the idea to start the EBS Optimizer project, which over time matured nicely and now offers a much more reliable, flexible and feature-rich conversion for EBS volumes, and is now our only solution now that this feature has been removed from AutoSpotting.

### Determine the Spot product automatically (correctness/flexibility/reliability/simplification)

AutoSpotting used to have the On-Demand pricing hardcoded to the Linux/UNIX pricing, regardless which Operating System was used by the customer.

Even if a different Spot product was configured in CloudFormation, that was only used for the Spot pricing, and the On-Demand pricing was still used from Linux/Unix everywhere, including for instance type selection but also for billing.

This lead to reduced diversification of such configurations and incorrect billing when used with non-Linux/UNIX instances.

The new version of AutoSpotting determines this information automatically for each AutoScaling group and use it to have correct instance type selection and higher diversification when launching Spot instances, but also for ensuring correct billing, which is very important for our Enterprise customers.

Also a single AutoSpotting installation will handle multiple Spot products in parallel across multiple AutoScaling groups, whereas previously you needed to run multiple installations side by side for covering multiple OSes at the same time, each with its own tag-based filtering.

This information is persisted as an ASG tag, and so far this tag will need to be updated manually or deleted when changing the AMI to another Spot Product (say from a Linux AMI to a Windows or RHEL AMI), otherwise the billing and instance type selection will be again incorrect.

In a future version we may update this tag on a regular basis, to avoid such issues.

This change makes the previous Spot Product configuration flag irrelevant, so it was deleted from the configuration of the CloudFormation and Terraform infrastructure code.

### Support installing AutoSpotting in any AWS region as its main region (cost/performance/correctness/flexibility)

Previously AutoSpotting only supported being installed in Virginia, and the Lambda function was performing calls across regions when customers had infrastructure elsewhere.

Since many of our customers run infrastructure in other regions, they often came with requests to install AutoSpotting next to their instances, so it wouldn't need to work across regions.

This worked for a while, but when we implemented AWS Marketplace and later with savings email reports, we again added a tight coupling to Virginia.

When attempting to install it in other regions it may have caused it to crash or fail in weird ways.

The current version supports deploying AutoSpotting in any AWS region as its main region, so you can run it where your instances are, for lower networking costs, and slightly faster execution.

Also the billing and other data we use for the savings email reports are hosted in the region where the AutoSpotting Lambda function is installed, while previously they were hardcoded to always use the us-east-1 region.

Note: going forward changing the region where AutoSpotting is installed will reset the cost savings history data that is used for the daily savings report emails.

### Simplify event forwarding across regions (reliability)

Historically AutoSpotting used Lambda functions deployed in all regions to collect events and then invoke the lambda function deployed in the main region(previously Virginia) with those events.

This was convoluted, brittle and relatively hard to maintain in the code. Events could be lost when invoking Lambda directly, especially at scale.

The current implementation uses the relatively new EventBridge cross-region event forwarding to forward the events to the SQS queue from the main region, which then invokes the Lambda directly. This has also a few architectural and workflow simplifications explained in more detail in this previous [blog](https://leanercloud.beehiiv.com/p/weekly-update-17-mar-2023).

Going forward events will be delivered reliably by EventBridge and stored in SQS until we process them. This offers increased reliability, and also the code base is more maintainable and the architecture simpler to reason with.

The regional Lambdas and ancillary resources have been also removed from the architecture and the code has been simplified.

This feature should be largely invisible to users.

### Consider UPDATE\_ROLLBACK\_FAILED as a valid steady CloudFormation stack state (correctness)

Previously the UPDATE\_ROLLBACK\_FAILED CloudFormation stack state was considered as in-progress, which made AutoSpotting reject to perform instance replacements for stacks in this state, just like it does for any instances created by CloudFormation stacks that are being updated.

This has been fixed in this release, so stacks in this states will be handled as a steady state and AutoSpotting will perform the usual instance replacement actions against their instances.

### Increase `InService` timeouts to up to 13min (reliability/correctness)

Some customers have instances that take a long time to start, for example because of lifecycle hooks that cause them to take long time to reach the InService state.

AutoSpotting used to wait for them for about a minute, which caused some of these instances to fail to be added to the AutoScaling groups.

The current version increases this timeout value to about 13 minutes, leaving some time until the AutoSpotting Lambda timeout at 15 minutes.

This is just a maximum value, in most cases we just wait for a few seconds.

### Reap unattached Spot instances running for more than 15min (cost/correctness)

This is related to the about timeouts, and makes sure we terminate any instances that may have resulted from such timeouts or any other situations that break the AutoSpotting replacement workflow, such as potential crashes during operation.

The main benefit is saving some costs in the rare events that the instance replacement workflow is interrupted for whatever reasons.

### Clean up the `Beanstalk Userdata patching` (simplification)

This piece of functionality was needed before we had the event-based instance replacement logic, and Spot instances were running for a while out of the AutoScaling groups.

It was essentially a workaround for being able to run CFN-Init before the instance was attached to the AutoScaling groups.

With the current event-based replacement architecture we attach Spot instances immediately after they are launched, so this feature is no longer needed.

This setting was deleted from the Infrastructure code, simplifying the configuration of AutoSpotting for new users.

### Dropping the `InstanceTerminationMethod` configuration flag (simplification)

This is another cleanup of a feature that is no longer needed, and deletion of its flag from the Infrastructure code, simplifying the configuration of AutoSpotting for new users.

### Avoid deleting the log group of the AutoSpotting Lambda function (logging/forensics)

A few releases ago we introduced a regression that deleted the logs of the AutoSpotting Lambda function when uninstalling or updating the CloudFormation template.

We now retain them in case we want to investigate the behavior of a previous version or installation of AutoSpotting.

## Getting started with this new version

You can install this version of AutoSpotting on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates), as usual, using CloudFormation and Terraform.

## Terraform code changes

Speaking of Terraform, over the last weeks I also revamped the Terraform code to bring it back in sync with CloudFormation, and also implemented a few enhancements documented below:

- ability to pass multiple email addresses to receive the daily report emails.
- avoid the need for double runs for new users - this is an improved workaround for one of the Terraform limitations regarding execution across multiple regions.
- support using an existing IAM role and VPC subnets, useful for customers with strict landing zones that block them from creating certain resources we need.
- support for passing a IAM permissions boundary to our IAM roles, again for large customers who may have such requirements from their Landing Zone.
- support for using trial builds from our public ECR for PoC scenarios.
- update dependencies, including the version of the AWS provider.

That's all for now, as I said this release was the largest in the entire history of AutoSpotting, and packed with lots of enhancements based on your feedback and suggestions.

Thank you for all the feedback please keep it coming, and stay tuned for further improvements going forward, which you can see on our public [roadmap](https://github.com/orgs/LeanerCloud/projects/1?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=leanercloud-product-updates).

Best regards,

Cristian, the author of AutoSpotting

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/leanercloud-product-updates/image-3.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


## Current OpenTofu contributors vs. pledged FTEs

OpenTofu Github stats half a year after the fork.


View more
