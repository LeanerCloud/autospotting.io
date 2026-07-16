---
title: "Another New AutoSpotting Release in Less Than a Week"
date: 2023-04-15T00:00:00+00:00
description: "AutoSpotting 1.2.1-0 addresses critical ECS load balancer draining issues during Spot terminations"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "ecs", "spot-instances", "load-balancer", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

A new version of AutoSpotting (1.2.1-0) addresses a critical issue affecting ECS users running Spot instances. The problem: "each Spot termination in their ECS cluster with Spot capacity managed by AutoSpotting resulted in dropped connections and 5xx errors."

## The Problem

ECS was slow triggering load balancer draining when Spot instances terminated, sometimes initiating the process only seconds before—or after—instance shutdown. This caused dropped connections and user-facing errors.

The issue affected all ECS users with Spot instances, not just AutoSpotting customers.

## The Solution

AutoSpotting now performs deregistration API calls within 10 seconds of detecting a Spot termination event. This gives ECS tasks sufficient time to drain connections cleanly.

Combined with immediate replacement Spot instance launches and OnDemand failover diversification, the solution reduces service disruption and maintains high availability.

## Updates

**Version 1.2.1-2** (released after initial 1.2.1-0) fixed handling of instances listening on dynamic ports rather than just the load balancer listener port.

## Installation

Available on [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6). For existing deployments, use this CloudFormation template with `SourceImageTag "stable-1.2.1-0"`:

```
https://s3.amazonaws.com/autospotting-builds/stable-1.2.1-0/template.yaml
```

## Images

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-2.png)

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-3.png)

![](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-4.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-5.png)

![Vantage just updated ec2instances.info and released all their code, now what?](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-6.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/another-new-autospotting-release-less-week/image-7.png)

