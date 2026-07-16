---
title: "Current OpenTofu Contributors vs. Pledged FTEs"
date: 2024-05-03T00:00:00+00:00
description: "Analyzing OpenTofu's actual development velocity against promised commitments"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "OpenTofu"]
tags: ["opentofu", "terraform", "open-source", "leanercloud"]
featured_image: "/images/blog/leanercloud/2024-05-03-opentofu-contributors.png"
draft: false
---

## OpenTofu Github stats half a year after the fork.


 May 03, 2024

After the recent layoffs at Google that impacted open source maintainers of some prominent projects, I was curious to see what's the state of other projects where the community had to step up involvement after the initial authors ceased their Open Source contributions.

I saw a few people made similar comparisons for other forked projects in the past. As an infra guy myself, I immediately thought about [OpenTofu](https://opentofu.org?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes), now that we're more than half a year after the fork from Terraform, they recently had a massive release, and also had some interesting shenanigans with Hashicorp's lawyers accusing them of stealing their precious IP.

## The Countless Pledges

Back when they forked the code, the OpenTofu website showed some 18 FTEs pledged by 4 companies to work full time on OpenTofu for at least 5 years, as you can see below.

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-2.png)

OpenTofu pledges shown at [https://opentofu.org/supporters/](https://opentofu.org/supporters/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes)

The list also included some 800 individuals and over 160 companies in total, most of them vaguely pledging *Development; open-source community efforts,* whatever that means.

When I first saw that last year I was sure they will crush Hashicorp when it comes to development velocity.

Now that 6 months have passed after the fork, I was curious how many of those pledges materialized in actual involvement in the project, or were they just trying to get backlinks to help their SEO.

## The reality

Thanks to Github, we can clearly compare the OpenTofu community effort with the pledges, and also with Hashicorp's development pace.

Looking at the project's [contributors](https://github.com/opentofu/opentofu/graphs/contributors?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes) graph on Github sadly only shows some 5 people actively contributing to OpenTofu at the moment, far from the 18 FTEs pledged not so long ago.

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-3.png)

We can see that at the beginning a few more people used to contribute, but unfortunately they soon either lost interest or were assigned to something else by their employers.

Now compare that with the same [graph](https://github.com/hashicorp/terraform/graphs/contributors?from=2023-08-01&to=2024-05-03&type=c&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes) for Hashicorp's Terraform repo:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-4.png)

I know that commits may not be the best way to compare projects like this(please suggest a different metric) but to me it seems like Hashicorp still invests more resources than the entire OpenTofu community combined.

Now, there are also a few other projects under the OpenTofu Github Organization. The vast majority are provider repos, largely maintained by the cloud providers, and kept largely in sync with their Hashicorp fork origins.

The main exception under active development is the Registry repo, for which you can see the similar graph below:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-5.png)

The same pattern can be seen there as well, a bunch of people contributed at the beginning, only to stop their involvement after a few months. Looking over the last month and a half we can see these 4 people, most of them with just a handful of commits:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-6.png)

And now let's see a similar graph of the last few weeks for the main OpenTofu repo:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-7.png)

I'm not trying to judge anyone's productivity, but the way I see it, a FTE developer should be able to churn out a bit more than a couple of commits in a month and a half, and anyone below that seems to me more like a drive-by contributor than a pledged FTE.

## Pulse graphs

Then I got the idea to look at the Pulse graph over the last month, which shows this for the main OpenTofu repo, again seems like 6 people standing out from the pack of many drive-by contributors:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-8.png)

and here the same Pulse view for Terraform, which has some pretty active developers, at least when it comes to the number of commits, and about 9 in total standing out from the pack of drive-by contributors:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-9.png)

And the same graph for the OpenTofu registry repo that shows a single contributor but apparently for whatever reason the entire core team is consolidated under a single name:

![](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-10.png)

Out of all these I'm honestly a bit disappointed, but not surprised.

I've seen a very similar pattern for my own [AutoSpotting](https://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes) Community [Github repo](https://github.com/LeanerCloud/AutoSpotting?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes), which sadly hasn't seen any community contributions after I stopped contributing code changes about 2 years ago and decided to keep proprietary the many further [enhancements](https://github.com/LeanerCloud/AutoSpotting/discussions/489?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes) I made since.

My only hope from this post is that it will keep these companies accountable to contributing how much they pledged at the beginning and inspire more of those vague pledgers to take some action.

—

Discuss this on [HackerNews](https://news.ycombinator.com/item?id=40252620&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=current-opentofu-contributors-vs-pledged-ftes).

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/current-opentofu-contributors-vs-pledged-ftes/image-11.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## New AutoSpotting bugfix release - 1.3.1

Fixing regressions in 1.3.0: incompatibility with instances out of the DefaultVPC and Lambda timeout when replacing Spot instances


## Progress report for the first week after forking ec2instances.info


View more
