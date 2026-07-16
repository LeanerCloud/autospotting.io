---
title: "My Thoughts on the Current State of EC2 Spot Pricing"
seo_title: "The Current State of EC2 Spot Pricing"
date: 2023-05-10T00:00:00+00:00
description: "Understanding recent Spot price increases and practical strategies for maintaining cost savings"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Cost Optimization"]
tags: ["aws", "ec2", "spot-instances", "pricing", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-05-10-ec2-spot-pricing-thoughts.png"
draft: false
---

# My thoughts on the current state of EC2 Spot pricing

## And what you can do about it


 May 10, 2023

I've recently seen a [post](https://news.ycombinator.com/item?id=35802157&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing) on HackerNews discussing an interesting [article](https://pauley.me/post/2023/spot-price-trends/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing) titled “Farewell to the Era of Cheap EC2 Spot Instances”, painting a pretty depressing picture of recent trends of EC2 Spot pricing.

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-2.png)

As someone building AWS cost optimization [tooling](http://autospotting.io/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing) that makes it easy for people to use Spot instances, I'm pretty familiar with this space so thought I'd share my 2 cents about this current situation and wanted to also offer a few suggestions to Spot users who want to still get the most of Spot instances.

I've also seen this first hand during my tests for the recent AutoSpotting releases, and also asked by AutoSpotting customers as soon as this first became apparent. To be honest I didn't really have an answer for them at the time, but I've been thinking about it and I think I have a few suggestions to make to Spot users who want to get decent Spot savings.

I'm also in a relatively tricky situation, I left AWS not so long ago so I know quite a lot about the Spot internals, capacity figures, and lots of things that I can't share without getting an army of AWS lawyers chasing me. I'll try to stay clear of releasing anything that's not public information and stick to the basics but at the same time trying to explain what’s going on and what you can do about it.

## How Spot works

In order to understand what's going on, I think it's better to explain in simple terms what Spot is and how it all works.

As you probably know by now, Spot is just spare capacity, not currently used by people using OnDemand, including when covered by RIs and Savings Plans.

AWS needs to always have some spare capacity for each instance type in order for you to be able to run OnDemand instances without Insufficient Capacity Errors(ICE). This is what you get as Spot.

There's no magic, just currently unused instances.

As a user of Spot instances, you also want some of this capacity to be available when you need to run instances, therefore some of it should always be unused by other Spot customers, and ideally it should be available at a heavily discounted price, so it worth using Spot in the first place.

In simple terms, for each instance type in an availability zone(so called capacity pool), you have:

Total\_Capacity = On\_Demand + Spot

and

Spot = Spot\_Running + Spot\_Free

These are the capacity figures, not the costs, and to simplify OnDemand also include capacity covered by Reserved Instances and Savings Plans, which are applied after the fact on your bill.

From my times at AWS, I remember the typical Spot utilization figures(the ratio between Spot\_Running and the Spot total capacity), and even though I can't share any numbers, it’s obvious that AWS is incentivized to increase this Spot utilization number as much as possible because that's what brings them revenue from Spot users.

That's why they've been working hard to improve the experience of Spot customers over time and offering primitives that make it easier to run Spot wherever it's a great fit.

Such improvements are the EC2 Fleets, seamless integration with AutoScaling groups, Karpenter for EKS, but also the various capacity allocation strategies such as `capacity-optimized` and `price-capacity-optimized` that allow people to use Spot effectively for various use cases.

But as this utilization number increases for a given instance type, people see their Spot capacity getting interrupted more, which is detrimental to user experience.

Amazon really wants people to use Spot, and that ideally the experience to be as consistent as possible between instance types, so they like to avoid customers getting crazy levels of interruptions for some of their capacity pools while others are sitting idle.

Their way to spread out the load across their many instance types is by increasing the hourly price, in addition to the inherent increase in interruptions.

The idea is to encourage people to diversify across as many as possible instance types, and to offer automation that transitions between the capacity pools, like the capacity-optimized allocation strategy and its more recent variations.

## What's been going on lately?

For the last 5 years after the Spot bidding model was dropped, the pricing was pretty stable over time, with some seasonal increases in the Holiday Season.

Spot offers a great way to optimize costs, for suitable workloads often giving savings better then Savings Plans and RIs and without the upfront costs and long term commitments.

So with the current state of the global economy, as more and more people are doing cost optimization, and coupled with increasingly good tooling that makes it easier to adopt Spot, the last year’s Holiday Season capacity crunch became more of a constant state of affairs.

This was always the case for some specialized instance types, such as the GPU instances, where wide diversification is not possible, but it gradually propagated to many general purpose instance types, as you can see from the below screenshot taken from the [Spot Instance Advisor](https://aws.amazon.com/ec2/spot/instance-advisor/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing):

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-3.png)

## What can we do about it?

I can’t predict the future, but chances are we’ll eventually reach a state of equilibrium where Spot capacity becomes again available at a reasonable price over all.

And with over 600 instance types available at the moment, even though many of them are priced almost at the same level as OnDemand, with **enough** **diversification** you can still find plenty of instance types that still have decent savings. Here's how things look like at the other end of the savings spectrum:

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-4.png)

Yes, a lot of these are old instance types, have lots of interruptions and may eventually follow suit, but you can at least for now use them and still get decent savings.

The vast majority of common instance types still have savings in the 30-60% range, and many still have relatively low interruption frequencies:

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-5.png)

One thing you can do with plain AutoScaling groups is to **expand your diversification**, and use the new `price-capacity-optimized` allocation strategy. This will require some configuration changes to be rolled out across your fleet, but it's totally doable with reasonable effort. This will automatically tap into those lesser used instance types, and evening out the Spot utilization.

[AutoSpotting](https://AutoSpotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing) (including the Open Source Community Edition available on [GitHub](https://github.com/LeanerCloud/AutoSpotting?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=my-thoughts-on-the-current-state-of-ec2-spot-pricing)) makes this effortless, automatically diversifying across the widest possible range of instance types cheapest than your initial On-Demand instance type.

AutoSpotting also uses a custom prioritized allocation strategy that works much like the new `price-capacity-optimized` allocation strategy, but in addition also preferring recent instance types, for better performance and lower carbon footprint, as a little bonus to you and our planet.

Another thing you can do with AutoSpotting is **setting a hard limit on the savings percentage** you find acceptable or configuring the aggressive bidding policy we used back in the days of Spot bidding, which is still available.

This sort of configuration is also doable with plain Autoscaling groups but requires reconfiguration of every single group, while in AutoSpotting it's a global configuration option that can apply automatically throughout your entire account.

This allows you to set a limit at say 50% of savings, so that you never pay more than that, leaving more expensive capacity as OnDemand, so then you can **purchase Reserved Instances or Savings Plans** for it. Just make sure you pick a cutover point that makes sense depending on your Reserved Instances and Savings Plans coverage, since the savings cutover will depend heavily on the commitment and flexibility of your Savings Plans and Reserved Instances.

Yes, this won’t allow you to cover with Spot all instance types, especially fringe instance types like the smallest T4g.nano and GPU instance types, but it should work pretty well with mid-size instances where you have plenty of diversification potential.

Going forward I’ll be working on making AutoSpotting play nicer with Savings Plans and Reserved Instances, to automatically set this cutover point for each instance type, making it much easier for you to purchase RIs and Savings Plans for the remaining OnDemand capacity, and potentially also purchasing the reservations on your behalf to maximize the coverage of your remaining OnDemand capacity.

Stay tuned,

Cristian

#### Keep reading

[![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-6.png)

## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


## New AutoSpotting release, adding support for Mixed Autoscaling groups

How ECS capacity providers inspired me to fix an ancient AutoSpotting issue


## Why I recommended ECS instead of Kubernetes to my latest customer

And how a cost optimization exercise often leads to deeper modernization of cloud applications


View more
