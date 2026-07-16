---
title: "My Thoughts on the Current State of EC2 Spot Pricing"
date: 2023-05-10T00:00:00+00:00
description: "Understanding recent Spot price increases and practical strategies for maintaining cost savings"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Cost Optimization"]
tags: ["aws", "ec2", "spot-instances", "pricing", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-05-10-ec2-spot-pricing-thoughts.png"
draft: false
---

The author addresses recent concerns about rising EC2 Spot instance prices, offering perspective on what's driving these increases and practical strategies for users seeking to maintain cost savings.

## How Spot Capacity Works

Spot instances represent spare AWS capacity not currently consumed by on-demand users. The fundamental equation is straightforward: "Total_Capacity = On_Demand + Spot" and "Spot = Spot_Running + Spot_Free."

AWS maintains incentives to increase Spot utilization, driving improvements like EC2 Fleets and allocation strategies such as `capacity-optimized` and `price-capacity-optimized`. However, higher utilization means increased interruption rates.

## Recent Market Trends

For five years following the end of Spot bidding, pricing remained relatively stable with seasonal holiday fluctuations. Recently, this changed. As organizations prioritize cost optimization and tooling accessibility improves, Spot capacity constraints have become persistent rather than seasonal. GPU instances experienced this first; general-purpose instances followed suit.

## Practical Solutions

**Diversification remains key.** While many common instance types offer 30-60% savings with reasonable interruption rates, exploring over 600 available instance types reveals additional options at better prices.

**Configuration strategies include:**

- Using `price-capacity-optimized` allocation policies
- Implementing hard savings limits to reserve expensive capacity for Reserved Instances or Savings Plans
- Leveraging automation tools like AutoSpotting to manage diversification automatically

The author suggests equilibrium will eventually return, making aggressive diversification today's most viable cost optimization approach.

## Images

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-2.png)

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-3.png)

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-4.png)

![](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-5.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-6.png)

![New AutoSpotting release, adding support for Mixed Autoscaling groups](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-7.png)

![Why I recommended ECS instead of Kubernetes to my latest customer](/images/blog/leanercloud-content/thoughts-current-state-ec2-spot-pricing/image-8.png)

