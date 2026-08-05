---
robots: "index, follow"
title: "New AutoSpotting Releases"
date: 2024-04-11T00:00:00+00:00
description: "AutoSpotting 1.3.0 with new features and 1.2.3 with critical billing fix"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "spot-instances", "releases", "leanercloud"]
featured_image: "/images/blog/leanercloud/2024-04-11-new-autospotting-releases.png"
draft: false
---

After almost 8 months since the previous AutoSpotting release, we're happy to announce not one, but two AutoSpotting releases at the same time, packed with lots of improvements.

I was hoping to release this much earlier, but somehow each time we got quite close, customers reported yet another little but important edge case that would be be great to fix and then another and so on…

This finally resulted in a few **dozens of relatively small reliability, correctness and performance improvements** addressing lots of edge cases throughout the code base.

The list is massive, as you can see below, here are just the main ones:

- An important billing bugfix.
- Avoid reduced capacity during instance replacements.
- Configurable minimum Spot diversification within each Availability Zone for increase reliability
- Improved ECS task draining for Spot terminations for avoiding user visible errors
- Pause automatically during Beanstalk deployments and Autoscaling Instance Refresh actions
- Automatically migrate stateful resources like EBS volumes and Elastic IPs to replacement instances

..and many more you can read about below in further details.

AutoSpotting was an already mature product, in the works for more than 8 years now, but these improvements make it even better and we're very excited about this release.

Over the last few weeks it was relatively quiet so we decided to finally push the button and release AutoSpotting 1.3.0, which is now available on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases).

## **Pricing increase**

Considering our massive investment in improving AutoSpotting and the increased quality and reliability of the software, we also decided to start to increase prices in the new version, gradually getting closer to the market value for such tooling. For now **the savings cut will double from 5% to 10%**, which will allow us to invest even more in development, at the cost of a relatively little decrease of the savings you keep, from 95% to 90%.

We may gradually increase prices further going forward whenever the software improves significantly, but we **keep older versions available forever**, and even if we don't encourage it, **you may keep running your current version** for as long as you're happy with it.

We actually just helped a company using a 5 years old Open Source version of AutoSpotting onboard to this new version and they were still quite happy with the one they were using, but required some new functionality only available in the current version.

So without further ado, let's dive right in:

## **Version 1.2.3 - bugfix release**

Right when we were about to release 1.3.0, we got a critical bug report in the previous stable version 1.2.2.

Considering the price increase, even though we hope the many improvements will still worth it for you to upgrade, we didn't want this bugfix to only be available in the new and costlier version.

That's why we decided to also cut a bugfix release that only includes this bugfix on top of the currently available stable version, so you can get this fix even if you don't want to upgrade yet to the next major version.

Previous versions of AutoSpotting were considering any running Spot instances when **calculating Spot savings, regardless if Spot instances were launched by AutoSpotting or not**. Since most AutoSpotting customers exclusively use AutoSpotting to adopt Spot instances, this was rarely a problem and nobody noticed yet.

A customer using an alternative solution for a part of their Spot capacity running a different kind of workload that doesn't run in ASGs noticed they were being overcharged and reported this issue, which we promptly fixed.

And while working with another customer we noticed Terraform customers were not billed at all, so while at it we also fixed billing in the Terraform code.

That's the only change in this minor release, so you can safely upgrade to get this fixed.

If you have other Spot workloads and notice lower AutoSpotting costs after updating to this version, please reach out to us on [Slack](https://join.slack.com/t/leanercloud/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases) in the *#support* channel to sort this out.

## **Version 1.3.0 - major feature release**

Besides the above billing bugfix, here is a summary of the many improvements from this new major release:

1. **Improve instance replacement logic to avoid decreased capacity** - AutoSpotting does instance replacements using Attach and Terminate API calls (with decreasing DesiredCapacity). Previously it used to default to terminating existing instances first, to avoid reaching maximum ASG capacity, and then calling the Attach API call.

   This resulted in temporarily decreased capacity, which was fine for as long as replacements were sequential, but became problematic in latest versions when multiple instances could be replaced in parallel by the event-based logic.

   Going forward, for as long as we're below the ASG maximum capacity we will first attach the new instances, then wait for the ASG grace period(with a 10min cap), before terminating the existing instance. This should make the entire replacement process more reliable.
2. **Support enforcing instance type diversification within an Availability Zone** **for increased reliability** - in the event of multiple concurrent Spot terminations, mainly for groups restricted to a single Availability Zone, we now support enforcing some level of instance type diversification within each Availability Zone.

   This is configurable with a new configuration flag in the CloudFormation template, named `InstanceTypesPerAZ` and the corresponding ASG tag `autospotting_number_of_instance_types_per_az` to override it.
3. **Improve ECS task draining reliability for terminating Spot instances** - Previously we used to simply drain all running ECS tasks from the terminating Spot instances. Some customers noticed that occasionally new tasks were still scheduled on the soon to be terminated instances, which resulted in errors.

   From this version we set ECS instance state to DRAINING instead of actively draining the tasks through API calls, still gracefully draining existing tasks but also ensuring that no new tasks are scheduled on terminating Spot instances, and achieving all this through fewer API calls and simplified logic.
4. **Awareness of Beanstalk deployments and Autoscaling Instance Refresh actions for reliable deployments without the need for deployment code workarounds** - Much like we already had for instances belonging to CloudFormation stacks in process of being updated, AutoSpotting will also avoid instance replacements while Elastic Beanstalk deployments and Autoscaling Instance Refresh actions are in progress.

   This caused deployment failures and required workarounds in the deployment code that temporarily suspended AutoSpotting execution to avoid interference with deployments. These workarounds should no longer be needed.
5. **Reduce SQS TTLs to avoid mass-processing of older events for more reliable operation -** We use an SQS queue under the hood, and while working with a new customer on a PoC they got an outage.

   Turned out they had manually disabled AutoSpotting execution for some issues encountered in their PoC, the queue accumulated events over multiple hours and then when they re-enabled AutoSpotting, it processed all these events at the same time, disrupting the group and causing a few minutes of outage.

   Going forward we reduce the SQS TimeToLive from 24h to 15 minutes to avoid such situations.
6. **Automatically migrate stateful resources like EBS volumes and Elastic IPs to replacement instances** - We're always trying to offload certain actions from external configuration, such as the early ECS task draining and ELB deregistration.

   When replacing instances we now also automatically migrate existing stateful resources attached to them like Elastic IPs and EBS volumes to the new instances, removing the need for this to be done in the application configuration.
7. **Disable Capacity Rebalancing** **for reduced instance charn in some situations** - This configuration may be set in some cases on new AutoScaling groups, and causes the launch of OnDemand instances and immediate termination of running Spot instances when receiving the Spot rebalancing event. This is interfering with the AutoSpotting instance replacement logic and causes increased instance churn.

   AutoSpotting handled this as churn gracefully as possible in the previous version, but we now also disable Capacity Rebalancing for the ASGs managed by AutoSpotting, avoiding the churn in the first place.
8. **Reap unattached OnDemand instances** - in rare situations when the replacement logic is interrupted for whatever reasons(like maybe in case of rate limiting on large AWS accounts or software crashes), instances may remain lying around outside the ASGs and costing money.

   A while back we started reaping such Spot instances to avoid their extra costs, and going forward we'll also reap OnDemand instances that may be launched as failover to insufficient Spot capacity.
9. **Reuse Launch Templates converted from Launch Configurations only within the first minute and reap them after a week** - Under the hood we launch Spot instances with EC2 Fleet API calls, which require Launch Templates. In case of ASGs using Launch Templates we just reuse them, but for ASGs using Launch Configurations, we convert the existing Launch Configuration into a Launch Template.

   These converted Launch Templates used be deleted immediately, but then at some point we realized keeping them for longer may be useful for troubleshooting. So in a previous version we started reusing them indefinitely, which caused problems with stale application versions, or using old and potentially insecure AMIs or different instance types than in the current configuration.

   Going forward we only reuse converted Launch Templates within the same minute, such as in case we replace multiple instances in parallel, and we keep them for a week for troubleshooting purposes, but reaping older ones in order to not have too many lying around in your account.
10. **Conversion of small EBS volumes to GP3 for ASGs using Launch Configurations** - Since Launch Configurations need to be converted to Launch Templates, this allows us to slightly change the configuration.

    We previously had support for converting their EBS volumes to GP3, for 20% cost saving for storage and more predictable storage performance. This was removed at some point in the past when we converted to EC2 Fleets and Launch Templates, but we brought back the support while trying to restore some old Launch Configuration code for a bugfix mentioned below.

    This is now done automatically for volumes below 170GB where there are both cost and performance benefits from the conversion to GP3.
11. **Updated instance type information** - we can now cover all instance types released as of end of February 2024.
12. **Converted to SDK for Go v2** - for increased performance, reduced memory consumption and future-proofing the code base we converted the entire source code to SDK-Go-v2 and revamped many internal components of the code base in the process.
13. **Decreased memory consumption** - actively worked to further reduce memory consumption and improve scalability by passing most internal data structures by reference throughout the code base, which got us from about 450MB to around 150MB, and should improve scalability for large customers.
14. **Updated Go version** - for increased security and better performance we now automatically use the latest version of Go when building AutoSpotting.
15. **Lots of reliability and code quality enhancements** - fixed about a dozen crashes that used to happen in certain rare conditions.
16. **Passing all gosec static code checks** - implemented dozens of under the hood code improvements, mostly for correctness, better logging and error handling throughout the entire code base, as you can see in the below screenshots.

    ![](/images/blog/leanercloud-content/new-autospotting-releases/image-2.png)

    AutoSpotting 1.2.3

    ![](/images/blog/leanercloud-content/new-autospotting-releases/image-3.png)

    AutoSpotting 1.3.0

    The numbers had become even worse while working on this release, especially after the huge changes required to upgrade to SDK for Go v2, when one of the correctness issues caused a strange bug, so after fixing that bug we decided to fix all these static checks this version.

### **Bug fixes**

1. **Billing bugfixes -** the same billing fixes mentioned for 1.2.3 are also included in this release.
2. **Fix support for EC2 Classic** **configuration style with SecurityGroups given by name instead of by ID** - One of our older Open Source customers noticed that support for Launch Configurations written initially for EC2 Classic, with Security Groups passed by name instead of ID, was broken in the latest versions.

   After converting to EC2 Fleets we need to use Security Group IDs, so we now determine the Security Group IDs automatically when given by name to keep supporting such configurations.

   The same customer also had the same configuration style in Launch Templates, so we also made a similar fix for these.
3. **Fix concurrent Lambda execution while handling Spot Termination events** - the Spot termination event handling used to block the processing of events from our SQS FIFO queue, so no other actions would be handled until the Spot termination event handling would be completed.

   We now immediately delete all events from SQS after we start processing them, allowing for other events to be handled in parallel by other Lambda invocations.

## **How to upgrade**

AutoSpotting is available on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases), you can use the instructions from the Marketplace to install it if you don't have it already, or to update to either 1.2.3 or 1.3.0, as you see fit.

And if you're looking to optimize your AWS costs and are open to get external help that would offload much of the optimization work from your engineers, at [LeanerCloud](https://LeanerCloud.com?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases) we also offer comprehensive cloud optimization services that cover the entire AWS offering, also available through the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-abvqqdkbxpe4q?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases) for easier procurement.

That's all folks, thanks for reading this far and as always, feel free to reach out on [Slack](https://join.slack.com/t/leanercloud/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=new-autospotting-releases) if you have any questions.

-Cristian

#### Keep reading

[![How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD](/images/blog/leanercloud-content/new-autospotting-releases/image-4.png)

## How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD


## How we maximized task density on our ECS cluster by avoiding burstable instances

And why we had to deploy NAT Gateways in our public subnets


## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


View more
