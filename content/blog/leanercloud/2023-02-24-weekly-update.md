---
title: "Weekly Update - 24 Feb 2023"
date: 2023-02-24T00:00:00+00:00
description: "Customer onboarding challenges, parallel instance replacement breakthrough, and marketplace automation"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "marketplace", "performance", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

Hello, and welcome to this week's update.

Without further ado, let's dive right in.

## **Little recap from last week**

In my last week's update I mentioned how I've been working most of the week helping a new customer onboard.

They were reluctant to run the latest AutoSpotting version because of their blue-green deployment strategy, but they wanted to try an older version that still works in the legacy cron mode. They were initially very happy about the way I supported them to use an older version, and even said it was the best support they ever got from a vendor.

But unfortunately that older version I helped them run uses a lowest cost Spot allocation strategy makes it particularly prone to Spot interruptions. Unfortunately it ended up causing a couple of outages, which is the last thing you want when you try to onboard a new customer.

When looking into it we noticed the customer's configuration used only two of the 6 Availability Zones available in the Virginia region.

This, coupled with the lowest cost allocation strategy was a recipe for disaster.

Luckily I had built great relationship with the customer by giving them such a great support, so they still wanted to continue the PoC.

So I promised them that I'll make the latest version work for their use case. I was aware of these issues from other users, but nobody seemed to be bothered as much about them, so I thought it was fine.

Anyway, in the meantime they were going to give the old version another try but with all the 6 Availability Zones. That should work well, much like it does for so many other customers still using it.

## **Security issues with Alpine images**

Over the weekend they also noticed some security issues reported by their Docker image scanner when scanning the older Docker image, which was yet another hurdle to pass.

This is particularly frustrating considering that AutoSpotting is built as a static binary running in Lambda.

Besides the fact that there's a very small attack surface, we use use an Alpine base image only to to make the Marketplace Docker image scanner work.

The scanner doesn't support images build with "FROM scratch" and marks them as insecure. So our static binary doesn't use anything from that Alpine image, but then must use it, and then the fact that we use it makes us fail such security checks...

What a mess! 🤦🏻

So the first thing I tried to do was to get the same old AutoSpotting static binary and inject it into a newer Alpine image, and give it to the customer. I wanted to buy myself some time while they set it up with all the AZs from the region.

This still wasn't ideal, considering that their blue-green deployment strategy would have them run plenty of OnDemand instances during the day, since they deploy continuously, but it seemed like a good starting point.

## **Parallel instance replacements**

As I promised, I started working on making the new version perform instance replacements in parallel.

Before, AutoSpotting queued new instances and replaced them one by one, to keep the group's capacity steady.

So if you launched many instances at the same time, it could take some time. Then some instances started running their applications and added to load balancers, only to be terminated a few minutes later.

This churn was not noticeable by most users. But this customer has a blue-green deployment style which relies on doubling the capacity, so this took them longer to deploy their software. This would cause them to have lots of OnDemand instances running during the day, when they have frequent deployments.

They also run some commercial observability software billing a full hour of runtime for those couple of minutes, increasing their software costs.

Throughout Monday I kept testing it to understand what's going on and kept running into issues. I then continued on Tuesday, when I finally figured it out somehow.

Once I figured it out it worked like a charm, with the only caveat that we needed to double the ASG Max capacity to make it work.

That was completely doable by this customer, since anyway they use a blue-green deployment method. They're already doubling capacity for each deployment, so they just had to raise the max capacity from 2x to 3x, which was pretty easy.

It wasn't ideal, but still it was much better than before. So I decided to release this on the AWS Marketplace, but off by default, just in case someone else may benefit from it.

## **First Marketplace release**

Before I cut the release, I also wanted to update the instance type information. This is an easy process of updating the dependencies, getting the version of ec2-instances-info I had been working on last week.

This version also includes the auto-update functionality, which I also tested and improved a bit more, but still decided to turn off by default.

Together with the parallel instance replacement progress made this already a significant release.

## **AWS Marketplace tooling work**

To be honest I always dreaded cutting new software releases, mainly because of the clunky GUI of the AWS Marketplace management portal.

For those of you who haven't been yet lucky enough to enjoy this beautiful piece of engineering, it's full of small fields containing Markdown text, having to copy/paste entries manually from the previous release and so on.

Every time I ended up spending hours doing it manually, tweaking it again and again after plenty of human errors. Seems as if the AWS Marketplace team never tried to use their own GUI, but I digress...

It kind of felt like using a blunt saw for cutting a tree,

so I decided to spend some time "sharpening the saw", as they say.

Even if the GUI wasn't so bad, I always wanted to have a way to cut releases from code. Ideally I'd have something much like the [https://github.com/elias5000/clouds-aws](https://github.com/elias5000/clouds-aws?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-24-feb-2023) tool, inspired by one of my first tools I built for AWS back in early 2014.

At first I wanted to start with some other tools, but couldn't find anything decent, only some hackish proof of concept snippets using the AWS CLI, nowhere near the polished tool I wanted to have.

So with help from ChatGPT I started to build such a tool, at least to be able to avoid that copy-paste error-prone messy process every single time.

I complained about the inconvenient Marketplace GUI, but once I saw the API of the Marketplace I felt like crying and quitting it all.

Think of Go structs that pass a blob of escaped JSON, with hardly documented fields, which I had to reverse engineer from scratch.

I gave myself Wednesday to work on it, and by the afternoon I had some basic working code able to update the product information, which was encouraging.

I used it to update the AutoSpotting product entry, [released](https://github.com/LeanerCloud/aws-marketplace-cli?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-24-feb-2023) the tool's source code as Open Source and called it a day.

On Thursday I continued working on it for a few hours, hoping I'd be able to get it cut the new AutoSpotting release with it.

Getting it work was very elusive, so I ended up cutting the release manually, and I decided to put it on hold, until the next release, whenever that' will be.

## **Second AutoSpotting breakthrough**

On Thursday afternoon I joined an event at the AWS office, in the same building where I used to work until 6 months ago.

It was a great session organized by another vendor in the cost optimization space, which would be a great synergy for AutoSpotting and EBS Optimizer.

After the event, while riding the bike on my way back home I got a pretty simple idea on how to solve the problem with the double maximum capacity.

Once I got home I implemented it pretty fast, and then it worked like a charm when tested.

With this latest code, within 10-20 seconds you usually get the replacement Spot instances. The OnDemand instances are all terminated, usually within less than 2 minutes from their launch, and all this without the need to bump the Max capacity, there are just a couple of additional instances temporarily increasing the ASG capacity and decreasing automatically when the replacement actions are over.

In most cases OnDemand instances shouldn't get enough time to get configured with such proprietary software, added to the load balancer, and also the time it takes to do blue/green deployments would stay about the same.

I felt like I finally found the holy grail of instance replacements, and the customer soon tested it and was very happy with how it worked.

This was definitely worthy of another release, so I decided to cut one today.

## **More Marketplace tooling work**

Before the release, I wanted to spend a few more hours on the Marketplace tooling, hoping I could finally get it work and use it for cutting the release.

So I continued working on the Marketplace tool throughout most of Friday.

Using some input from the CloudTrail event generated when cutting yesterday's release I managed to get it work, and I'm sure it will save me a lot of time and energy for my following releases. One less source of papercuts!

I used it to cut the new release of AutoSpotting, which is the one now available on the AWS Marketplace.

It took like 5 minutes, without any error-prone clicking and copy/paste, no reluctance whatsoever, it was actually a joy to see it work so well. I swear it's not the Ikea effect, it's a much better experience 😀

There's also room to make it even better going forward, so I'll keep improving it bit by bit before cutting each new release, but it's already working great for my needs.

Besides this new functionality of cutting releases, I also did a massive refactoring thanks to ChatGPT. Now the code is of much better quality, and other Marketplace vendors will hopefully start contributing to it to help with some of the remaining polishing work.

You can check it out [here](https://github.com/LeanerCloud/aws-marketplace-cli?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-24-feb-2023).

## **Bulk EBS volume tagging**

If you remember from last week, I contributed support for bulk-tagging AutoScaling groups to [awstaghelper](https://github.com/mpostument/awstaghelper?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-24-feb-2023), an Open Source bulk-tagging tool.

I did this so you can easily apply tags when getting started with AutoSpotting, to reduce a bit the adoption friction.

Over the last weekend I extended support also for EBS volumes. This contribution was pretty much built by ChatGPT, and in record time, took me less than half an hour.

The new GUI will also soon remove a lot of the friction for new users when tagging resources and quickly tweaking AutoSpotting configurations. Also, we'll have the interactive cost savings estimates.

But this command-line tool will be a decent alternative for DevOps or SREs, who are more inclined to use a command-line tool, much like I prefered a CLI tool over the Marketplace GUI.

## **Plans for next week**

My main plan for next week is to focus on the GUI, adding support for configuring most of the common tags, and releasing a first version of it.

Then once the GUI is in out of the door, I will start spending some time on marketing and sales, since the last few weeks were pretty much dedicated to product development.

With help from ChatGPT I may still code my way through some of it, like using the Github scraper I built the other week for outreach to people from the community, but not to the extent of what I've built over the last few weeks.

That's pretty much it for now, see you again next week.

**One more thing**: If you're using AutoSpotting do yourself a favor and try the latest version, I bet you'll like it way more than before, but let me know what you think about it. You can get it from [here](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-24-feb-2023).

#### Keep reading

[![Why I just declined a job offer from a billionaire friend](/images/blog/leanercloud-content/weekly-update-24-feb-2023/image-2.png)

## Why I just declined a job offer from a billionaire friend


## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


## Progress report for the first week after forking ec2instances.info


View more
