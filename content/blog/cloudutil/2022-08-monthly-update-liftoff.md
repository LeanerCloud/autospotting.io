---
title: "Monthly Update August 2022 - Liftoff"
date: 2022-09-10T20:36:12+01:00
description: "Going full-time on cloud cost optimization, new AutoSpotting stable release, and LeanerCloud launch"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting", "LeanerCloud"]
tags: ["monthly-update", "autospotting", "aws", "cloudutil", "marketplace", "leanercloud"]
draft: false
---

Hello,

I'm happy to announce that I finally left my full-time job last week, so this is my first status update as
self-employed. Over the last few weeks I've been vacationing a lot So far there's not much to share

It's been a while since my previous update in here, which was at the end of
October.

I'm now trying to resume these monthly updates, so in this first post after
this gap I'd like to give a few updates covering the last months, and also my
plans going forward.

## Personal updates

I've decided to finally leave my cushy full-time job and to start working
full-time on a new cloud cost optimization business built around AutoSpotting
and EBS Optimizer, starting from September.

There are many things I'll have to figure out as I go, but for now the plan is
to offer support for optimizing cloud costs, including hands-on and ideally by
leveraging my tools whenever possible. I'm not going to be religious about
the tooling, I'm open to using anything that makes sense for the customer, and
only using my own tools if it they make sense for the customer and if they help me
accelerate my work.

I'll also be looking into building additional tools that automate more of my
work, and open to partnering with various SaaS providers and resellers in the
FinOps and cloud cost optimization space.

This means I'll also have more time to work on AutoSpotting and EBS Optimizer
on a regular basis, but at the same time that I'll have to spend a good chunk
of my time doing delivery, marketing and sales.

I'm incredibly excited but also honestly quite scared about this journey, and I'd
really appreciate any help I could get. It would be great if you could refer me
within your companies, or if you could start using AutoSpotting and EBS
Optimizer from the AWS
[Marketplace](https://aws.amazon.com/marketplace/seller-profile?id=a7ef2f5c-28b4-4dc2-90f5-ce4da9127c7f)
instead of building them from source code.

Please reach out to me on
[Slack](https://join.slack.com/t/cloudutil/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg)
if you have any questions or suggestions on how we could work together.

## AutoSpotting

In my previous [blog](//monthly-update-october-2021/) I mentioned that I've
been working on a large refactoring that changes the way we launch Spot
instances, which offers the ability to get better uptimes. This landed in the
Github open source repo sometime in October and I've been working on
stabilizing it since. For many months I was quite busy with other things and
didn't get to work much on AutoSpotting so I made little progress on this workstream.

At the same time I wanted to launch a new stable release of AutoSpotting, since
the previous release was a long time ago and it desperately needed some instance
type information updates. I was reluctant to also include this new
functionality, so instead I decided to build a new stable release using the
previous logic, with just a few instance type information updates and a few bug
fixes.

This release took much longer than I expected, but I'm happy to announce that we
finally got a stable release of AutoSpotting that delivers these changes. This
is as usual available on the AWS
[Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6).

In addition to the instance type updates which now cover all instance types
released up to June 2022, this release also changes the way we perform instance
replacements when first using AutoSpotting on an existing AWS account. This
shouldn't affect existing users in any way, but on new installations existing
OnDemand instances are now simply terminated one by one at a pace of one
instance every 30 minutes, leaving the Group to replace them with new OnDemand
instances, which are then immediately replaced by the AutoSpotting's
event-based replacement logic.

Previously these instances were cloned by new Spot instances, which would then
be running outside of the Autoscaling group for a while and replaced using a
more complex workflow. This workflow was problematic, for example when the
Autoscaling group would be deleted, these instances were causing failures to
delete the group. The workflow was also somewhat buggy and hard to get right,
and at times it was launching Spot  instances that were failing to be
attached to the group and just running outside, wasting money.

I therefore decided to simplify this workflow by just terminating one existing OnDemand
instance at a time, and having only the usual event-based logic used for all instance
replacements.

Now that this release is out, I will resume working on the main branch,
continuing the work on maturing the change that landed back in October.

My plan is that the next stable release will include this functionality,
especially considering the fact that I'll be working more consistently on
AutoSpotting going forward.

## EBS Optimizer

No additional changes over the last months.

## Websites

- Back in November I performed a few changes on
[autospotting.io](https://autospotting.io), mainly further cleaning up the copy
and adding new testimonials.
- [cloudutil.io](https://cloudutil.io) has also
seen a few copy changes and cleanups, let me know what you think about it.
- I'm working on a new website which will
 at some point become live at [leanercloud.com](https://leanercloud.com), and will cover my new cost
optimization business.

My existing websites will remain live for the forseeable future, with largely
the same kind of content.

## Income report

My Marketplace income has been relatively flat, at around $150/month from 6
subscribers, the vast majority for AutoSpotting. Patreon still brings about
$200/month from 7 subscribers, so in total I'm making some $350/month. If
you're still using AutoSpotting from Patreon, please consider switching to
using it from the AWS Marketplace, there are tons of improvements.

I'd like to extend a warm "Thank you!" to all of you who are purchasing
AutoSpotting supported builds either via Patreon or the Marketplace, your continued
support motivates me to keep improving it further.

If you are working on a large company that might be interested in working with
me once I'm on my own, please reach out.

When it comes to expenses, besides my AWS costs of about $15/month, over the
last few months I've been working with someone to help me with sales and
marketing, paying some $200/month for his services. If you notice any emails
from felix@autospotting.org, or messages from him on our Slack room, that's him
at work.

## Plans for September and beyond

- Stable Marketplace release that includes the recent AutoSpotting features.
- Add more docs on [cloudutil.io](https://cloudutil.io) about both EBS Optimizer
  and AutoSpotting
- Proactive Spot-to-Spot instance replacements in the event of instance
  rebalancing notifications, with ICE-proof on-demand failover across multiple
  instance types

I'm incredibly excited about the future and I'll keep sharing my
progress monthly as I work on them, so stay tuned for future updates.

I'm always eager to hearing from you, so feel free to reach out if you have any
questions, feedback, or need any help. Just ping me on
[Slack](https://join.slack.com/t/cloudutil/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg) and I'll personally
answer every single message I get from you.

Cristian
