---
title: "Weekly Update - 17 Feb 2023"
date: 2023-02-17T00:00:00+00:00
description: "Re-launching AutoSpotting OSS, instance type data improvements, and Reserved Instances awareness"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "open-source", "community-edition", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

Welcome to this week's progress update.

This was a shorter week than usual, as Monday had some family emergency and largely took the day off, but it was otherwise a pretty productive week, with many things moving forward and a lot of things to write about, so this is probably my longest update so far, but more technical than usual.

It was again a week focused on development work, worked on multiple things but still on the same themes of reducing friction for new users and making life easier for existing users.

### Re-launching AutoSpotting OSS as Community Edition

After my last week's [recording](https://www.loom.com/share/cfc08ec635b743da99769b5a62c5f482?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) of the new GUI for AutoSpotting in which I ran into an AutoSpotting bug that stopped me from doing an end-to-end demo, this week I finally got to it and fixed that bug in AutoSpotting.

It was a simple [one-liner](https://github.com/LeanerCloud/AutoSpotting/commit/547618137ec8346c2431bb7a1d5814253a7957ff?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) fix, but then this fix got me thinking on the future of the AutoSpotting Open Source project.

As you may know by now, most the functionality I've been working on since September, when I left AWS and doubled down on AutoSpotting isn't open source and only available to my Marketplace users in binary form.

Back in September I had written a warning on the [AutoSpotting](https://github.com/LeanerCloud/AutoSpotting?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) README that the public open source code is no longer being updated, pointing to a post in which I explained what I'm trying to do and telling people to use AutoSpotting from the AWS Marketplace.

In retrospect that warning was a mistake, and thankfully one of the people I talked to recently explained the consequences and advised me to revive AutoSpotting open source development, and this bug fix was the perfect opportunity for that.

So I decided to delete that warning, made it more clear that AutoSpotting is an Open Core product, and articulated the [difference](https://github.com/LeanerCloud/AutoSpotting/discussions/489?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) between the Open Source code which I called AutoSpotting Community Edition and my current commercial offering.

In a nutshell, I'm going to keep focusing on building major new features in the commercial edition, helping my paying customers as much as possible, but I'll publish bug fixes and accept external contributions to the Community Edition. I may occasionally also release features in the Community Edition, but that remains to be seen on a case by case basis.

Ideally the delta between my Commercial version and the Community Edition will be kept to a minimum, but I think it needs to be significant enough to have at least some companies pay for it, so I can have a viable business and keep building things like this going forward.

I'll try to also articulate this on the [AutoSpotting.io](http://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) website and will start using the Github issue tracker for my roadmap for both AutoSpotting Community Edition and the commercial offering.

### Instance type data updates

On the same theme of improving the experience for my existing users, after feedback from many users over the years, this week I finally started working on adding support for updating the instance type information in AutoSpotting automatically.

For a little context, AutoSpotting needs instance type information such as CPU cores, amount of memory and on demand price in order to determine the specs of the running instances, and also in order to choose a range of suitable Spot instance types for replacing them.

This data is baked into the AutoSpotting binary, under the hood we use my [LeanerCloud/ec2-instances-info](https://github.com/LeanerCloud/ec2-instances-info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) library, which simply exposes the data from a convenient to use JSON file available on [https://instances.vantage.sh/](https://instances.vantage.sh/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) (as opposed to the horrendous AWS APIs offering the same data in a much harder to consume way).

This works great, and many users get started with AutoSpotting years ago, got going within a very little time and running the same old version for years, like this user said:

But then there are also some users who want to leverage newer instance types, which offer increased performance, especially since the recent versions of AutoSpotting prioritize new instance types.

This currently requires them to update AutoSpotting in order to get the new instance type data. This isn't so hard but still takes a little time and effort on their side, and they also get new functionality which sometimes differs from the previous version, bringing things they don't need or want.

I do my best to always improve AutoSpotting, and each version should improve things, but this need to update is not desirable.

I started by working on a new version of the ec2-instances-info library, implementing capability to download a new version of the JSON data file, and attempting to update the data if this file is correct.

After last week's experience with ChatGPT, in which I was able to build a pretty complex Github scraper script with just a few hours of work, I also extensively used ChatGPT for these changes to the ec2-instances-info library, and I was surprised how well it worked. It took me less than 2 solid hours of work to get the first version of that working.

Once I'm done with this, it should be a matter of having AutoSpotting update to the new version of the ec2-instances-info library, with minimal code changes in AutoSpotting itself, so stay tuned for the next AutoSpotting update which will include this improvement.

### Instance type data API

Related to this work, I tried to make the ec2-instances-info library not depend directly on the [Vantage](https://www.linkedin.com/company/vantage-sh/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) JSON URL at runtime (although they're kindly hosting this data and paying for the CDN traffic for it for years and I'm very thankful for that), but also to get more control and flexibility to point my users to another data file, maybe later with some modifications like deleting some of the fields what I don't need in order to save memory and bandwidth.

So also with lot of help from ChatGPT, within the same day I wrote a new API, which offers a download URL for the JSON data file:

For now it just returns the URL of the JSON file hosted by Vantage but later I may change it to host my own data with custom changes to make it smaller, etc.

Under the hood it's a Lambda function implemented in Go, CloudFront connecting to the new Lambda HTTP endpoint, ACM and Route53 for the custom domain and using DynamoDB for storing the API keys.

The function requires authentication with API keys because I plan to just use it for myself and don't want people to start depending on it, but anyone could in theory run their own data backends implementing the same API if they wanted to, or I may one day offer this as a service if people are willing to pay for it.

I'll soon prepare a new version of AutoSpotting that will use this updated ec2-instances-info library capable to refresh its instance type data from my API.

### Working on [ec2instances.info](http://ec2instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023)

While working on this functionality, I realized the upstream JSON data is not parsing correctly, and noticed that my own library had a patching [workaround](https://github.com/LeanerCloud/ec2-instances-info/blob/master/Makefile?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023#L21) that someone else had committed almost two years ago and I wasn't aware of it anymore.

I implemented the same workaround with a [2-liner](https://github.com/vantage-sh/ec2instances.info/commit/5e10db0686315ec5e46c8a38658f46a6107dc525?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) fix upstream, and also while at it I contributed a few [improvements](https://github.com/vantage-sh/ec2instances.info/pull/685?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) to their build system after struggling to run it locally after last time I had contributed to their repo more than a year ago.

This got me again in touch with my friends from Vantage who kindly offered to help with my data needs, and we're kicking off a collaboration on how to improve the situation of this instance type data.

### Twitch stream working on Reserved Instances and Savings Plan awareness

On Wednesday, I decided to take a little break from this data update work, and based on some feedback and things I remember from prior conversations with customers, I started to look into adding a checker for Reserved Instances and Savings Plans into AutoSpotting.

The idea is that larger customers usually have existing RIs and Savings plans covering their baseline capacity, and there are concerns that AutoSpotting may replace some of those instances with Spot, increasing the costs. This came again after a conversation with someone the other week in the context of the new GUI, so I wanted to somehow make the GUI aware of the RI and savings plan coverage, but then reuse the same information in AutoSpotting.

So also with help from ChatGPT, I started to write some logic that persists data about RI and Savings Plan coverage to a DynamoDB table, for further consumption. I wanted to share how it's done, so decided to share most of the process on a live Twitch stream. The Twitch session was only about 1h:20min, because I had to join a call, but even with that limited time I still got pretty far, and within less than 30min more later that day I got the new code to compile for the first time. It's still far from usable, but I'll keep iterating on it next week.

Unfortunately my network uplink struggled with a 5k Twitch stream and people experienced severe frame drops, but I'll look into reducing the screen resolution for subsequent streams and maybe even getting a better connection, since I kept experiencing issues with my current provider.

### OSS mass tagging tooling work

A couple of weeks ago, while working on the new GUI, I was brainstorming other ideas to improve the onboarding experience for new AutoSpotting and EBS Optimizer users. Maybe people who aren't fond of using a closed source GUI tool, but would rather use existing OSS tooling for mass-tagging their resources.

So in between working on the new GUI, I started to implement a change of the existing [awstaghelper](https://github.com/mpostument/awstaghelper?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) tool, that would allow it to handle AutoScaling groups. In between a couple of calls I got back to that code, did a few more improvements and contributed it upstream, and it was quickly merged, so now you can easily mass-tag your ASGs using the awstaghelper tool:

- If you have Go setup, it's as easy ar running

or getting a precompiled binary from their [releases](https://github.com/mpostument/awstaghelper/releases?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) page.

- Once you have it installed:

This will dump a list of all the ASGs into a CSV file that looks like this, notice I have no "spot-enabled" tags yet:

- Open it with an editor and add "true" after the trailing comma for each AutoScaling group that you want AutoSpotting to handle for you, let's say just for the second ASG in my example:

```
AutoScalingGroupName,spot-enabled
AutoScaling 2,
AutoSpottingDemo,true
```

- run awstaghelper again to apply the change to the CSV on all the selected ASGs:

```
awstaghelper asg tag-asg --filename asgTags.csv
```

- check the status in the AWS console

This should make things much easier for new users who won't have access to the GUI, which will only be available in the commercial AutoSpotting offering, but also maybe for commercial users who don't want to use the GUI.

Over the next few days I'll also add support for tagging EBS volumes.

### Helping AutoSpotting customers

A couple of days ago I was approached by a new customer who wanted to try AutoSpotting, but because of their deployment style was concerned about how the way AutoSpotting replaces instances would interfere with their deployments.

After a long conversation they decided to try an older version that still supports the old cron instance replacement mode and to disable the new event based logic which they were concerned about.

I helped them install this old version and to find a workaround for disabling the event based logic entirely.

Initially everything worked like a charm, and they were very happy with the way it worked and especially my support, enough to give me this raving feedback:

I also really loved working with them, we have great rapport and looking forward to see them succeed with AutoSpotting.

They also reported some issues with the Terraform code I wasn't aware of, in particular the fact that we were using old versions of the AWS and Docker providers.

I quickly [fixed](https://github.com/LeanerCloud/terraform-aws-autospotting/commit/0704393838cd382440bc47819b56ef4d0499190e?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-17-feb-2023) that, although the Terraform code still needs some work to catch up with CloudFormation, but I'll get to that eventually.

For now they creatively use Terraform to create the CloudFormation stack, and the workaround I suggested behaved as expected.

But unfortunately they soon started to experience increased terminations and sudden loss of capacity, which caused a 5min outage of their application and had them turn off AutoSpotting for now.

This is because the older version of AutoSpotting that can disable the event based logic uses a lowest-cost Spot allocation strategy, provisioning instances from the cheapest possible capacity pools, and then if those capacity pools are interrupted frequently, capacity can be lost. This shouldn't be a problem with the current version, but they won't use that because of their deployment style concerns.

So for now they disabled AutoSpotting entirely, until I address their deployment style concerns on the current version.

As I was writing this, it turned out they only used 2 Availability Zones out of the 6 in the Virginia region. They're going to give it another try once they start using all the AZs in the region. I expect them to not notice any issues once they start using all AZs.

### Plans for next week

My mantra is to always treat each customer as they're my only one, so this means that for the next week I'll be focusing on getting them going, maybe even improving the current version of AutoSpotting to accommodate their deployment needs (essentially achieving parallel instance replacements with the event based logic, as opposed to the current serialization through the SQS queue), so they can run with the latest version which should have much lower interruption rates. This shouldn't be so hard, as we used to do that in the past before adding the SQS queue, but let's see how that goes...

Once I'm done with that, the plan is to resume the uploader work, now that the upstream data is sorted out, releasing a first version of the GUI, which already got pretty good feedback from the people I asked so far, and then to look into the RI and Savings Plans coverage if there's any time left.

That's it for now, thanks for reading this far, and stay tuned for next week.

#### Keep reading

[![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/weekly-update-17-feb-2023/image-2.png)

## Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info


## New AutoSpotting bugfix release - 1.3.1

Fixing regressions in 1.3.0: incompatibility with instances out of the DefaultVPC and Lambda timeout when replacing Spot instances


## How we maximized task density on our ECS cluster by avoiding burstable instances

And why we had to deploy NAT Gateways in our public subnets


View more
