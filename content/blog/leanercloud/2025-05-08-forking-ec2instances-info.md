---
title: "Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info"
date: 2025-05-08T00:00:00+00:00
description: "Creating a community-driven fork of ec2instances.info after Vantage development stalled"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["ec2instances", "cloud-instances", "aws", "open-source", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-08-forking-ec2instances-info.png"
draft: false
---

Most people using AWS for enough time end up using the [ec2instances.info](http://ec2instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) website (a.k.a. instances.vantage.sh) to conveniently compare EC2 instance types across families based on specs and pricing.

It's basically an online spreadsheet with a lot of data about all cloud instance types and filters to slice and dice that data, as you can see below:

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-2.png)

## The ec2instances.info Open Source project

Few users realize that this very useful website is backed by an open source project.

The project itself was started back in 2011 by Garret ‘[powdahound](https://github.com/powdahound?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info)' Heaton, and has evolved a lot over the years.

Besides the convenient UI, under the hood it also has a comprehensive and easy to use database of instance type specs and pricing for many AWS services (including EC2, RDS, ElastiCache, etc.) that are much easier to use and more comprehensive than the official AWS APIs.

When in 2016 I started to build what's currently known as [AutoSpotting.io](https://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info), I was already using it a lot and noticed this database would be very useful for AutoSpotting so I started to use it, and it's still in use in the current version of AutoSpotting and many of my other cost optimization tools.

I soon found issues that needed to be fixed, so I became increasingly involved in the development work, until at some point Garret added me as a co-maintainer.

At the time there was no native AWS API for instance type specs, so we were scraping a lot of HTML tables to compile this specs database.

That made the scraper very brittle and since it was always very popular within the AWS community and got a lot of traffic, lots of people reported issues with it all the time so it took a lot of work to maintain it.

## Selling the website to Vantage

Garret eventually got busy with other things and back in 2021 decided to sell the website to Vantage, at the time an emerging player in the FinOps space.

I wasn't aware about the sale until after the fact, and at first felt a bit upset about it, considering my involvement at the time.

But then the Vantage CEO, Ben Schaechter, reached out to me to reassure me that they plan to keep it maintained as open source and to only use it as a low-touch marketing asset. Vantage had just launched, so taking over this popular website made them into a prominent player in the AWS cost visibility space virtually overnight.

They were well funded and since the website was such a useful asset, they assigned an engineer, named Everett Berry, to work on it consistently. Everett made countless improvements, including a redesign that made it look much slicker, lots of additional data fields and so much more.

## My involvement in the project after the sale

For a while I remained in constant communication with Ben, having monthly catch-up calls. But at the time I was working at AWS and was quite busy, so I didn't have time to get as involved in development, only occasionally contributed changes to it, such as the Spot support as part of my job of Specialist Solution Architect for Spot.

After leaving AWS to become a cloud optimization specialist I got to use it a lot more and came up with a bunch more changes, such as the Valkey support for Elasticache, SQL Server pricing for RDS, docker-compose usability fixes, etc.

I also increasingly started to use the instance type database in a variety of tools I build as part of my cloud optimization work, using my own [ec2-instances-info](https://github.com/LeanerCloud/ec2-instances-info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) Go library that I extracted out of AutoSpotting at some point to make this data easy to consume for other use cases.

## Vantage stops development

After a few years of massive progress under the new Vantage ownership, Everett left Vantage about a year ago and development all but stopped.

I kept using it and occasionally contributed a bunch of bugfixes and some of the features I mentioned above.

People kept using it and reporting issues all the time, so Brooke McKim, the Vantage CTO tried to step up for a while, but his last commits were in October 2024, and afterwards I was the only contributor to the project.

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-3.png)

## Vantage decides to rewrite the website from scratch

Back in March, Ben, the Vantage CEO announced on LinkedIn that they're looking for freelancers to help with the website after they hired a new marketing director.

Considering my long involvement in the project, I clearly expressed my interest and thought it would be a great opportunity to improve it further under their sponsorship.

They reached out and asked me for my hourly rate and I replied with a retainer offer that I thought would make sense for both parties.

It wasn't cheap, but a small fraction of their pay range for their open jobs in New York, so I thought it was a no-brainer for them considering my unique experience in this space and previous involvement in the project.

I haven't heard from them for more than a month, so last week I sent a follow-up email that was also ignored, so earlier this week I reached out to Ben directly on their Slack channel.

Apparently they found me too costly and said they found a couple of lower cost freelancers who are currently working on rewriting the website from scratch at a fraction of what I asked them.

## My decision to fork the project

To make it clear, I don't do this because they rejected me and picked those other freelancers. I currently have two gigs at the same time that bring me good money and keep me busy enough.

But I got very concerned about the direction of the project.

As I said before, I use it daily for my work and I also heavily depend on the instance type data source for many of my tools.

Just the other week I built a new Azure VM rightsizing tool based on that data source, and I have a bunch more tools I built previously for things like RDS rightsizing, automating the Aurora I/O Optimized configuration, etc.

It's also not clear how the rewritten website will look like, whether it will support the same functionality and instance specs database that I'm relying on so much in my daily work.

Then, even assuming the new version remains fully compatible, as any rewrite, chances are it will take time for it to mature, introducing potentially new bugs while the community keeps reporting issue and sending pull requests that Vantage isn't very responsive in addressing.

I can't just take the chance that they break these things consider that I rely so much on it for my day to day work.

So the other day I decided to fork it and immediately told Ben on Slack and started to prepare the infrastructure for the fork.

## What we have so far

It's been just a couple of days since I decided to fork it, but here's what we have so far:

- A new repo forked on Github, [https://github.com/LeanerCloud/cloud-instances.info](https://github.com/LeanerCloud/cloud-instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info), with issues and discussions enabled. If you raised any issues on the Vantage repo I'll do my best to address them over the upcoming weeks, but going forward please raise them here.
- Automated builds using a new Github Action that pushes the static content to a S3 compatible bucket hosted on CloudFlare R2. This took me a lot of time but it seems to work well so far.
- Code changes that enable the CI/CD builds, as well as cleaning up the code of Vantage logos and other copyrighted/trademarked materials.
- A [Slack room](https://join.slack.com/t/leanercloud/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) in my own Slack organization where people can engage with the rest of the community.
- A new website([https://cloud-instances.info/](https://cloud-instances.info/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info)) also hosted on CloudFlare, with the required domain and DNS configuration, that exposes that R2 bucket through the CloudFlare CDN. I did my best to ensure that everything works as expected but please [report Github issues](https://github.com/LeanerCloud/cloud-instances.info/issues/new?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) if you notice anything broken

(Later edit: if you get a 404 when opening the above link just delete the Beehiiv query strings which currently break it because of the way we serve it from the static content bucket, then it should work. I'll look into automating this ASAP.)

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-4.png)

## What's missing from the Vantage version

- I did my best to clean up the code of Vantage logos, links to their Slack and marketing calls to action, keeping it plain and simple as you can see in the above screenshot.
- Turns out the Azure support code hasn't been open sourced by Vantage, but also on their website the UI is broken and nobody fixed it in more than a year. I asked Ben if they’re planning top open source that work, but he rejected considering that I would use them in this competitive fork. So unfortunately we'll need to rewrite that. I'll look into that over the next few days.

## What are my goals going forward

I want this fork to become once again vendor neutral, well maintained and fully developed in the open with the rest of the community like it used to be for many years before Vantage acquired it.

So if you're using it regularly you're more than welcome to join the fun.

I also plan to rewrite from scratch the Azure support code with even more data fields and without the bugs they have on the Vantage site, then to expand it further to cover additional cloud providers at the level of richness we have for AWS at the moment, and feel free to let me know if you have other ideas.

Contributions from anyone are more than welcome, so please report any issues, feedback and suggestions on [Github](https://github.com/LeanerCloud/cloud-instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) or [Slack](https://join.slack.com/t/leanercloud/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info).

Let's rock and roll! 🚀

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-5.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Vantage just updated ec2instances.info and released all their code, now what?


## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


View more
