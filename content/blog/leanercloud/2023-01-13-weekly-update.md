---
title: "Weekly Update - 13 Jan 2023"
date: 2023-01-13T00:00:00+00:00
description: "LinkedIn outreach, EBS Optimizer release, podcast appearance, and savings calculator launch"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "EBS-Optimizer"]
tags: ["weekly-update", "linkedin", "ebs-optimizer", "podcast", "savings-calculator", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

Hello, and welcome to my latest weekly status update.

This week was another good week for me, I'm very happy about my progress.

## LinkedIn Outreach

I've recently started experimenting with cold outreach to drive some more sales, and I've been iterating on my messages.

Over the last weekend I asked for some feedback in one of the online communities I'm in, where I started a thread in which I asked how to make my messages less spammy and intrusive.

By the end it turned out into a veritable marketing course with almost 50 replies from many people :-)

So Monday I spent a few hours tweaking my messaging, but also reaching out and talking to people.

## EBS Optimizer release

Tuesday I worked on a new EBS Optimizer release, the second one since I first published it on the AWS Marketplace, and a quite significant one. [Get it](https://aws.amazon.com/marketplace/pp/prodview-ryzl67mmq3ghk?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-jan-2023) while it's hot.

This release brings a lot of benefits when it comes to controlling the actions of EBS Optimizer, requested by many people who tried it out.

EBS Optimizer is now pretty much as flexible as AutoSpotting when it comes to the control part. We now have opt-out mode, tag based filtering and control for conversion of IO1/IO2 volumes.

This release also changes the billing model of EBS optimizer, now a better fit for its one-off resource conversion model.

To get an idea, AutoSpotting needs to be running to avoid decaying Spot environments to On Demand instances. But EBS Optimizer could be installed for a few minutes, having the conversion done and then uninstalled with minimal charges.

Also, its billing model based on hourly savings was offering a very generous free tier of at least 700GB or sometimes 1.5TB.

That wasn't intentional, but it resulted from the prices of EBS volumes, combined with charging the same 5% for consistency with AutoSpotting and AWS Marketplace billing granularity.

So out of about a dozen signups on the AWS Marketplace for EBS Optimizer, there are now no paying users. They could be running within the huge free tier, or installed and then removed soon after. Or even not installed at all out of fear because the rollout couldn't be controlled before.

But this release fixes all these issues. Besides implementing the rollout control functionality, it also removes the free tier.

I also got feedback from many people that I'm charging too little. So I'm using this change to also increase pricing a bit. From the previous 5% billed hourly we now charge for each volume one month worth of savings right when converting it. That becomes about 8.33% with amortization over an entire year. And then each year we reset and charge again to make it recurring.

This is still a really low price point, so I guess that at this price point a free tier isn't quite necessary. But looking forward to see what users have to say about it.

You can get this new EBS Optimizer release from the [AWS Marketplace.](https://aws.amazon.com/marketplace/pp/prodview-ryzl67mmq3ghk?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-jan-2023)

## Podcast guest interview

Besides the EBS Optimizer release, on Tuesday I also recorded a podcast interview. It's not for my own podcast, but as a guest for Jon Myer's podcast.

I met Jon in the AWS Fest re:invent re:cap session about a month ago, and he was actually the one who inspired me to start my own podcast.

A few weeks ago I reached out to Jon and told him I'd like to talk in his podcast, and I'm happy that he agreed. As the saying goes, "ask and you shall receive" :-)

We had a great conversation about AWS cost savings and my ideas on cloud optimization. We also talked about the value of learning by doing versus studying for AWS certifications without hands-on experience, audience building, and how I'm using online communities and ChatGPT for my work.

After we finished the recording Jon also shared a lot of great podcasting tips that will help me going forward with my own podcast. I wish that was also part of it, but I'm looking forward to testing them in my next episode and may cover them later.

I can't wait to see this episode go live, should be ready in a couple of weeks or so. Make sure you subscribe to Jon's [podcast](https://www.youtube.com/channel/UCYHTwdOBne3eH1UFySCmtlQ?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-jan-2023) so you don't miss it when it's ready.

## Potential partnerships

Tuesday I also had a conversation with a former colleague, interested in a business partnership. We talked for a lot of time, and he gave me a lot of ideas on how to improve my marketing and website content. I'm looking forward to see how this turns out.

I was planning for the rest of the week to resume the work on the GUI I was working on last year. But out of feedback from him and another former colleague, I decided to change plans, so instead I started to implement some of his advice.

Over the rest of the week I also kept talking to people, bouncing back ideas and discussing potential collaborations. As I said before, I enjoy this part of my work a lot and looking forward what will come out of it.

## Savings Calculator

From the call with this former colleague I got a huge list of things to improve. I started with a few small [website](https://leanercloud.com?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-jan-2023) tweaks, but the main one I got to actually do this week was building a savings calculator for my tools. This is from the same theme of improving visibility into cost savings I mentioned a few weeks ago and seemed like a great thing to do.

This savings calculator also implements in a simpler and easier to build way a few of the functions I would have done in the GUI. It's also available immediately to customers and prospects, while the GUI will take me some more time.

I'll still build the GUI but it's not so burning urgent now that I have this savings calculator. I can then focus it on the rollout control part, not the cost estimation, which I now have from this calculator.

Although it would be nice for it to use real data from the AWS account and to have it coupled with the rollout control functionality, like the GUI will do it, I'd say it's good enough for now. We I'll still get there eventually, stay tuned for more work in this space.

Initially it only included AutoSpotting savings based on a list of instance types. But then I also added a mode that uses the AWS costs. For this I use some typical percentages of Spot adoption I've seen at customers. I also then built a similar mode for EBS Optimizer.

At least for AutoSpotting, the calculation includes the current Spot pricing information and also average Spot savings based on it. It's not perfect, but should be good enough to get an idea of the savings.

Many users requested this sort of savings estimation, long overdue, after so many years of building AutoSpotting. So I'm very happy I finally made it and how well it turned out for an initial release.

You can check it out [here](https://docs.google.com/spreadsheets/d/1xSZhAZdeikzBcqv7QnL-speepHW_1aaqJzYT5UHUz-Y/edit?usp=sharing&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-13-jan-2023), let me know if you have any feedback about it, and I'm happy to iterate based on feedback.

I'll also soon link it on my new website, and give a screenshot as a teaser for it, as part of the offering.

## Win: daily income record

And to share a little win, as I was writing this update I got the last daily business report.

If I'm not mistaken, today was likely my record daily revenue, at least over the last couple of months, if not all-time, with over $20/day. If this continues at the same level I'm on track to grow my AutoSpotting revenue over the $600 this month.

![](/images/blog/leanercloud-content/weekly-update-13-jan-2023/image-2.png)

As you can see, more than half of it from my biggest user, who's going to save some $6k this month. I find this a bit scary to have such a high percentage of my income coming from a single user, but also nice to see how much people are saving with my tools.

I never saw any numbers from my OSS version, it probably a few orders of magnitude more at peak, but now at least I can see these figures and I see growth.

(BTW I wish I could see a nice graph of my daily income numbers over the last few months, instead of having to open CSVs in Excel. I may eventually build that based on the code I mentioned I wrote last week with help from ChatGPT.)

For a bit of context, during the holiday season my revenue had dropped a lot. In November and December I've got almost to half the figures from previous months, at around $250-$300 each month.

But I expected this because Spot capacity is usually quite tight in the holiday season each year. Back when I worked at AWS, this time of the year was very tough for me as a Spot specialist SA, with quite a few people having capacity issues. This year also I've seen people using the native ASG Spot integration weren't as lucky and had to stop using Spot:

"Spot capacity has gotten really bad this year. I've moved all my stuff to on-demand SPs instead." - someone on the AWS reddit

But none of my users complained about insufficient capacity issues this year. Even if I made less money, it turns out the fallback to Spot worked like a charm also this year, which is great to see. Another little win, if I may say it like that.

So yeah, that's all for this week.

As always, I had plans to do a few more things, but I'm very happy with how this week turned out. The new EBS Optimizer release and the savings calculator are huge steps forward for me!

Have a great weekend and stay tuned for my next status update. I haven't yet decided what I'll do next week, maybe I'll finally start working on the GUI...

-Cristian

#### Keep reading

[![New AutoSpotting release, adding support for Mixed Autoscaling groups](/images/blog/leanercloud-content/weekly-update-13-jan-2023/image-3.png)

## New AutoSpotting release, adding support for Mixed Autoscaling groups

How ECS capacity providers inspired me to fix an ancient AutoSpotting issue


## Why I recommended ECS instead of Kubernetes to my latest customer

And how a cost optimization exercise often leads to deeper modernization of cloud applications


## How we maximized task density on our ECS cluster by avoiding burstable instances

And why we had to deploy NAT Gateways in our public subnets


View more
