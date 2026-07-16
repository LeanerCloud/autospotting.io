---
title: "Weekly Update - 13 Mar 2023"
date: 2023-03-13T00:00:00+00:00
description: "Sales initiatives, GitHub maintenance, infrastructure optimization, and customer onboarding progress"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "sales", "github", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.jpg"
draft: false
---

Hello and welcome to my latest weekly progress report.

I’m a bit late to this week’s update, I normally do these on Fridays as a way for me to reflect on the previous week, but on Friday I was busy helping a new potentially large AutoSpotting customer get started.

That didn’t leave me enough time to write this, but I’ll talk about the opportunity and the work itself towards the end of this progress report.

## Sales and Marketing

As I mentioned in my previous update, after an intense development sprint over the last few weeks, I’m now focusing more on sales and marketing, or at least trying to spend more than half of my time on such activities.

I’ve started last week by reaching out to previous contacts, mainly people who used AutoSpotting before, but also decision makers from companies who may benefit from my tools and services. This lead me to a number of conversations and I’m already pursuing a few opportunities of collaboration.

## Github issue triage and dependabot setup

One of the ways for me to find people to reach out to was by going through the Github issues of the AutoSpotting Community Edition. I spent some time closing things that are already done, looking for things to do next and commenting on issues that still may be relevant and trying so see if the people who raised them are still interested in them.

All in all I closed about a third of the total issues, bringing the numbers from 45 to about 30 issues and initiated conversations with a number of former and existing users.

While at it I also set up dependabot, which is now raising pull requests for dependency updates, keeping updated helps avoid potential security issues.

The private fork I distribute through the AWS Marketplace is up-to-date, but the Open Source code in the Community Edition is a bit behind. I’m planning to update it in my next development sprint and rebase my private changes on top of it.

## Streamlining my test setup

I’m always keeping an eye on my own cloud costs and trying to run a lean operation. In my last AWS bill, I saw a cost increase on my AWS test environment.

I was expecting this, since when building the latest version of AutoSpotting and Savings Estimator I’ve been spinning up and down lots of instances to make sure instance replacements work well. Also, some of them were a bit larger than usual in order to be able to show Savings some meaningful numbers in the Savings Estimator.

As a result of this my AWS costs increased a bit over the previous month and I also noticed some unexpected charges on AWS Config caused by this heavy instance churn.

After looking into this I converted my test setup to use T4g.small instances, which are available as part of a special free tier until the end of the year, and also figured out how to disable AWS Config, which wasn’t so easy as it’s enforced as part of my ControlTower setup.

## More Open Source work

On Wednesday I used ChatGPT to build a little serverless tool that sends me by email the CSV files dropped by the AWS Marketplace in my S3 bucket, which I built to save me time when trying to read those reports, which are very useful for my prospecting work.

I then released it on [Github](https://github.com/LeanerCloud/terraform-aws-email-files-dropped-in-s3?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) and also published it as a [module](https://registry.terraform.io/modules/LeanerCloud/email-files-dropped-in-s3/aws/latest?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) on the Terraform registry just in case someone else out there may find it useful.

This was the 3rd week in a row in which I released a new open source project, after the [AWS Marketplace CLI](https://github.com/LeanerCloud/aws-marketplace-cli?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) and [Savings Estimator](https://github.com/LeanerCloud/savings-estimator?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) projects that I released in the previous weeks, on top of the many Open Source contributions a number of other existing projects I did over the last few weeks.

I’m actually trying to release as much as possible of my software as Open Source, especially such small helper tools. I do it in order to give back to the Open Source community that brought me so many benefits, but also as a way to have a wider reach and hopefully get more paying customers to AutoSpotting, like it was the case when I open sourced the Savings Estimator and my contributions to the [awstaghelper](https://github.com/mpostument/awstaghelper/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) bulk tag helper tool.

I’m only keeping proprietary a few bits which I deem critical for monetization, for example AutoSpotting is still mostly Open Source, with the exception of a few usability, performance and reliability [enhancements](https://github.com/LeanerCloud/AutoSpotting/discussions/489?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) I only include in the commercial edition I sell on the AWS Marketplace, and EBS Optimizer is open source except for the tag-based filtering feature I released back in January.

My streak of weekly Open Source releases didn’t go unnoticed, as I was writing this I saw the [AWS open source newsletter](https://dev.to/aws/aws-open-source-newsletter-148-5h2b?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) mention my Open Source projects for the 3rd week in a row, and I got in touch with their editors to also appear in their podcast in a few weeks, stay tuned for that and also for more Open Source releases.

## Helping a new customer adopt AutoSpotting

As I mentioned at the beginning, I’ve been recently approached by a pretty well known company that is interested in adopting AutoSpotting at a meaningful large scale, with potential to become larger in revenue than all my other paying customers combined.

I’ve been talking to them a while back, while I was still working at AWS, but they ran into some IAM permission issues when installing AutoSpotting, caused by their restricted Landing Zone.

Back then I didn’t have time to help them overcome them but they got back to me again, and this time I promissed I’d do my best to help them get started, just like I do with most of my new users who ask for help.

So on Friday, instead of writing this progress report I had a call with one of their engineers, trying to understand what’s going on and how I can help them overcome these issues and started to work to make AutoSpotting support such restricted environments, which leads me to...

## Terraform code enhancements

After that conversation I started building a new feature in the AutoSpotting Terraform infrastructure code, adding support for using existing IAM role and also running inside existing subnets so that we don’t require Landing Zone changes or installing AutoSpotting with escalated permissions.

I soon got it work, but then I also took the opportunity to refactor the AutoSpotting Terraform code, making the regional event collector infrastructure use EventBridge instead of a Lambda function for forwarding the events to the main region, which is something I’ve wanted to do for a long time, and would help simplify the setup for such a restricted scenario.

I also got that working over the weekend, but soon noticed some parallel instance replacement issues which I’m now working to address. These shouldn’t matter for this customer, but for whatever reasons parallel replacements can only do to 2 instances at a time when using the EventBridge for cross-region event forwarding.

Out of this interaction I’m also looking into improving the billing of Windows EC2 instances, automating the Spot Product selection and enabling AutoSpotting to be deployed in any region.

## Testimonial

Last week I also received a new testimonial by a new AutoSpotting user.

I’m particularly happy about this one because it comes from a new customer who’s now my second largest, and also was given by a former Amazonian, who tend to have quite a high bar when it comes to customer support expectations.

![](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-2.png)

I really did my best to help them get started, first offering a workaround of using an older version until I get to implement parallel instance replacement now available in the current version, and then working with them to help them update to the latest version.

Out of this interaction with them I also got to bring the Terraform AutoSpotting module up-to-date and on par with CloudFormation.

But as you can see, I’m always trying to improve my product and address rough edges discovered by new prospects, just like I’m now doing with the Windows support, automated Spot product handling and EventBridge enhancements I’m building as a result of the work with my current prospect.

## EBS Optimizer repositioning and new joint venture

Last week I also decided to reposition [EBS Optimizer](https://aws.amazon.com/marketplace/pp/prodview-ryzl67mmq3ghk?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-mar-2023) as a mainly way to conveniently take care of any remnants of a manual conversion of GP2 to GP3 volumes, since many people attempt to perform the conversion on their own, only to notice they can’t figure out how to convert some of the volumes.

With EBS Optimizer you can get done with all that within just a few minutes and get back to your main work instead of chasing configurations creating new GP2 volumes.

I will also join forces with a fellow ex-Amazonian, who will join me in working on the EBS Optimizer, and potentially other products going forward.

This way I can focus on AutoSpotting for the foreseeable future, while he’s taking over EBS Optimizer, driving a number of improvements that will hopefully make it a more appealing to potential customers.

I’m very excited about this collaboration, and stay tuned for a few exciting updates from the EBS Optimizer front going forward.

## Wrapping up and plans for this week

That’s pretty much all about last week, all in all I’m very happy with the progress and looking forward to keep up this momentum.

For this week I’m planning to continue focusing on the sales and marketing, but also helping the onboarding of this new AutoSpotting customer and also preparing a talk at a company here in Berlin on Friday, which I’m very excited about.

That’s if for now, have a great week and stay tuned for the next update, probably again to come next Monday.

-Cristian

#### Keep reading

[![Progress report for the first week after forking ec2instances.info](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-3.png)

## Progress report for the first week after forking ec2instances.info


## Vantage just updated ec2instances.info and released all their code, now what?


## Why I just declined a job offer from a billionaire friend


View more
