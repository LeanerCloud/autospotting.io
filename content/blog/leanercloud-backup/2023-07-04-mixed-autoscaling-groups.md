---
title: "New AutoSpotting Release: Mixed Autoscaling Groups Support"
date: 2023-07-04T00:00:00+00:00
description: "AutoSpotting now supports AWS AutoScaling groups with Mixed Instances Policy"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "AWS", "Releases"]
tags: ["autospotting", "aws", "autoscaling", "spot-instances", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-07-04-mixed-autoscaling-groups.png"
draft: false
---

AutoSpotting announced a significant update enabling support for AWS AutoScaling groups configured with Mixed Instances Policy. The tool facilitates adoption of Spot instances in existing autoscaling infrastructure "without requiring configuration changes, by replacing their instances with Spot clones" through API operations.

## Key Updates

### What's New

The release addresses a long-standing limitation: AutoSpotting previously ignored groups using Mixed Instances Policy configurations to prevent race conditions. Now the tool manages these groups seamlessly without manual adjustments.

Additionally, the update fixes a regression preventing conversion of existing instances to Spot, restoring gradual instance replacement functionality.

### Why It Matters

Mixed Instances Policy groups—available since 2018—have become standard, particularly for EKS node groups. This feature was the "main impediment to adoption of AutoSpotting by new users lately," as the tool's previous requirement to reconfigure groups contradicted its zero-configuration promise.

## Technical Implementation

The solution draws inspiration from ECS capacity providers' Scale In protection mechanism. AutoSpotting now:

- **Applies ScaleIn protection** to all launched Spot instances, preventing termination during policy enforcement
- **Terminates replacement OnDemand instances** that autoscaling automatically launches when attempting to maintain configured ratios
- **Monitors capacity periodically** (default: 30-minute intervals) to adjust protection settings during scale-down events

## Deployment

Users can update via CloudFormation or Terraform using version `stable-1.2.2-0`. The update supports gradual adoption with rollback capabilities if issues arise.

## Images

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-2.png)

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-3.png)

![](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-4.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-5.png)

![Why I just declined a job offer from a billionaire friend](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-6.png)

![New AutoSpotting releases ](/images/blog/leanercloud-content/new-autospotting-release-adding-support-mixed-autoscaling-groups/image-7.png)

