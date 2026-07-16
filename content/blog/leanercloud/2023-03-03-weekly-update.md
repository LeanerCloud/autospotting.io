---
title: "Weekly Update - 3 Mar 2023"
date: 2023-03-03T00:00:00+00:00
description: "Terraform updates, Spot Savings Estimator launch, and website improvements"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "terraform", "savings-estimator", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

Hello and welcome to this week's progress report.

This is also a bit of a special one, since it marks more than 6 months since I left AWS to double down on AutoSpotting development.

Without further ado, let's dive right in!

### Terraform updates

I started the week by working on bringing the AutoSpotting Terraform module in sync with the CloudFormation template, after previously it was rolling out an ancient release of AutoSpotting from October 2021, with lots of known issues.

We now deploy the latest version of AutoSpotting available on the AWS Marketplace and expose all the recent features.

In some ways we even have a few enhancements, such as when configuring the email reports we can pass a list of email addresses instead of just one currently possible in CloudFormation.

I also improved the way the Terraform code runs under the hood, to avoid the need to run Terraform twice for the vast majority of users.

The only situation in which you still need the double run is when using AutoSpotting in AWS regions that require opt-in, which should be rare.

So if you've been using AutoSpotting installed from Terraform, you should definitely check out this release from the [Terraform Registry](https://registry.terraform.io/modules/AutoSpotting/autospotting/aws/latest?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-3-mar-2023).

### AutoSpotting GUI now launched as [Spot Savings Estimator](https://github.com/LeanerCloud/savings-estimator?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-3-mar-2023)

This week I also released the GUI tool I've been working over the last few weeks.

Here's a demo of it in action, showing how it integrates with AutoSpotting:

Initially the plan for it was to be the main GUI for AutoSpotting and EBS Optimizer and to keep it as a closed source component, exclusive to AutoSpotting and EBS Optimizer users.

But as I've been talking to various people I heard again and again that especially DevOps engineers find it easier to use open source tools, especially if it comes to giving the tool access to their AWS account.

They're also more forgiving when it comes to potential issues in a first release of such a tool, and may even contribute to such it going forward.

That's why I decided to pivot and release it as an Open Source Spot cost savings estimation tool that works independent of AutoSpotting but can be used to conveniently configure AutoSpotting by applying a its configuration tags if you want to adopt Spot quickly and more reliably than doing the configuration manually.

This allows me to also widen the scope of the tool and hopefully get more users into the funnel, out of which some will hopefully convert to paying AutoSpotting users to save time and get a more reliable setup.

From the functionality perspective it does the same thing, and I have the same roadmap for it, it's just that I'll be developing it in the open.

So far the Savings Estimator was relatively well received, after I announced it on a number of communities the Github repo stats reports 882 Clones from 246 Unique cloners and 534 Views from 115 Unique visitors.

I'm very excited about this tool and how easy it makes it to adopt Spot with all the best practices powered by AutoSpotting in just a few minutes.

### Website updates

For the rest of the week I've been working on updating the [autospotting.io](http://autospotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-3-mar-2023) website and the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-3-mar-2023) copy to mention the Savings Estimator tool as a convenient way to get started with AutoSpotting.

As part of this I also did a few small changes to my [AWS Marketplace tool](https://github.com/LeanerCloud/aws-marketplace-cli?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-3-mar-2023) I released the other week, which already saved me some time when updating the copy for the AWS Marketplace.

### Wrapping up

That's pretty much all for this week, I'm very happy with the progress, and how so many pieces of the puzzle started to get in place lately.

Now with the Savings Estimator, the updated Terraform code and the previous week's AutoSpotting release with parallel instance replacements, I have a pretty solid offering which saves people both money and time when adopting Spot.

After many weeks with heavy focus on development, I'll now be focusing on marketing and sales for the next few weeks, trying to get these amazing tools in the hands of as many users as possible, and then use their feedback to inform further development.

So if you know anyone who may benefit from my tools, please sent them my way and I'm more than happy to help them.

-Cristian

#### Keep reading

[![New AutoSpotting releases ](/images/blog/leanercloud-content/weekly-update-3-mar-2023/image-2.png)

## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


## How we maximized task density on our ECS cluster by avoiding burstable instances

And why we had to deploy NAT Gateways in our public subnets


## How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD


View more
