---
title: "Weekly Update - 17 Mar 2023"
date: 2023-03-17T00:00:00+00:00
description: "AutoSpotting architecture improvements driven by large enterprise customer onboarding"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "architecture", "eventbridge", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.jpg"
draft: false
---

Hello and welcome to this week's status update.

Let's dive tight in!

## AutoSpotting Architecture changes as result of my current customer onboarding

This week was another one largely focused on AutoSpotting development, most of the work resulting from the conversation I had with the relatively large enterprise customer I started to onboard last week to AutoSpotting.

This is high priority work for me, since their PoC alone would be bigger than all my other customers combined, so I did my best to make it work for them.

As I mentioned before, this wasn't the first time they tried AutoSpotting, but their restrictive Landing Zone configuration was the same and we again ran into a lot of errors.

But maybe I should explain the current architecture of AutoSpotting to get an idea what I've been trying to do:

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-2.png)

AutoSpotting is a serverless application mainly implemented in Lambda, and entirely event driven. The main Lambda function has always been written in Go and this architecture evolved a lot since the project was started back in 2016.

Initially it was all driven by a single cron rule that replaced instances one at a time.

Currently the Lambda function reacts to all sorts of events such as new instance launches, Spot instance terminations, and even Lifecycle hooks.

There is still a cron rule that runs it every 30min to reap/replace OnDemand instances but that's the exception, only used for initial replacements on new installations or in the unlikely cases when we fail to obtain Spot capacity across all the diversified capacity pools.

Since the EC2 instances can be launched in any number of regions and the Lambda is installed only once in the main region, we need a way to forward those events to the main Lambda function.

Historically that was done by a small Lambda function written in Python, which invokes the main Lambda function directly.

The main Lambda function then sends itself those events serialized through a FIFO SQS queue. The Queue then invokes the Lambda again for each event in order to process them one at a time, to avoid messing up the ASGs with too many attach/detach API calls, like when creating a new ASG.

When the Lambda gets an event, it usually connects to the originating region and takes actions there, usually launching Spot instances or swapping them with OnDemand instances.

Because of AWS Marketplace limitations, there is also a Fargate cron job that runs hourly for sending the billing information to the Marketplace API. In turn that requires a VPC and a bunch of ancillary resources.

Because unfortunately Lambda can't use an external ECR registry, we also have an ECR registry used for storing the Docker image used by both Fargate and Lambda, which both uses the same Container image downloaded from the AWS Marketplace.

We also use SSM parameter store for storing data such as houlry/daily/monthly savings figures, and an SNS topic is used for sending daily savings reports emails.

The Lambdas and Fargate have a restricted set of IAM permissions defined in a role with attached IAM policy

To get back to the customer, during the call with them last week, I took a lot of notes on things to address in our infrastructure code, such as making the Terraform code accept an existing IAM role and existing VPC subnets, among other things.

Once I started to go down this rabbit hole, it also turned out some of the Terraform modules which we were using were creating internal IAM roles and attaching policies, which was again blocked.

After a couple more calls with their engineer, I eventually figured out that their LandingZone configuration doesn't allow creating IAM roles or attaching IAM policies.

It also blocks us from creating VPCs(which I unfortunately need for the billing code

So I made the AutoSpotting Terraform code optionally use existing IAM roles, subnets and Permission Boundaries, with some help from ChatGPT on converting the modules into distinct resources.

I also used this opportunity to look into simplifying a bit the architecture of event forwarding, at least when deployed from Terraform code, something I wanted to do for a long time.

In the Terraform code we now replaced the regional Lambda event forwarder functions that invoke the main Lambda function with cross-region EventBridge rules, and also the IAM configuration and VPC can be optionally reused from existing configurations predefined in the user's AWS account.

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-3.png)

Besides this, I also added support for shipping Docker images through my own public ECR repository, which allows people to use AutoSpotting for a trial run before purchasing it through the AWS Marketplace.

These rules still invoke the Lambda for now, which keep the Lambda/SQS loop, but the plan is to have them send events directly to the SQS queue, to further simplify the AutoSpotting code and avoid that message loop between Lambda and SQS, which is what I'll be working on next:

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-4.png)

These code changes will be at first implemented in Terraform simply because that's what the current customer is using and I want to finish that first, but I'll eventually port them to CloudFormation. The terraform code still hasn't been tagged yet for this, but I'll cut a Terraform release once I reach the final state, hopefully sometimes next week, then I'll work on doing the same in CloudFormation, since most of my users use CloudFormation.

I've also been recently in touch with the AWS Marketplace team to see if we can somehow avoid the need to use Fargate and a local ECR repo, but that's probably going to take some time.

I'm also working with them to see if we can use Arm64 Docker images, preferably built using `FROM scratch`, which I tried in the past but wasn't working, hope that's going to work sooner.

## Progress on the PoC

After all these the customer was able to run the AutoSpotting PoC and got some instances replaced, but soon discovered some issues with a bunch of Spot instances being launched outside their ASG.

They run Windows, which needs a lot of time to boot, so their instances were timing out the AutoSpotting Lambda, which only waited for a minute until instances were marked as In Service in the ASG, and then the Lambda didn't finish the lifecycle of attaching them to the ASG.

I soon addressed the issue by increasing the timeouts, but going forward I plan to add another event rule for the InService event, to avoid waiting from within the Lambda function call.

All in all I'm happy with how this interaction improved AutoSpotting a lot under the hood and provides more feedback for a few more improvements going forward.

In spite of all the issues we uncovered, the customer is still happy with the way I supported them and quickly addressed the issues and very excited to see the massive projected savings materialize on their next AWS bill.

## Presentation of AutoSpotting and Savings Estimator

Besides these changes, I've also prepared and delivered a talk on AutoSpotting at a company where one of my former colleagues is currently working.

It was a generic introduction to Spot instances much like I used to have with the largest customers back when I worked at AWS, but it also included a short demo of using the Savings Estimator, which I used to prepare configuration for AutoSpotting, then running AutoSpotting with that configuration.

The talk was well received and as far as I know it was recorded, so if I can also get the recording I'll ask for approval to share it publicly.

If I won't be able to get the recording and you're interested in having such talks at your company, I'm more than happy to present it again.

That's it for now, stay tuned for more exciting changes in next week's update.

-Cristi

#### Keep reading

[![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-5.png)

## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


## Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info


View more
