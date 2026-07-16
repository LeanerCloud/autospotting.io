---
title: "LeanerCloud Product Updates"
date: 2023-04-12T00:00:00+00:00
description: "AutoSpotting 1.2.0-2 delivers the most substantial release in 7 years with major performance and reliability enhancements"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "spot-instances", "updates", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

This marks the most substantial release in AutoSpotting's 7-year history, delivering over a dozen enhancements across performance, reliability, correctness, and usability.

## Major Enhancements

### Proactive Spot Instance Launch

The system now responds to Spot termination events by immediately launching replacement Spot instances, eliminating costly temporary On-Demand failovers that previously delayed capacity recovery by approximately one minute.

### Diversified On-Demand Failover

When Spot capacity proves unavailable, the tool attempts launching On-Demand instances across multiple instance types rather than relying on single-type configurations, providing improved resilience against Insufficient Capacity Errors.

### Automatic Spot Product Detection

The release removes hardcoded Linux/UNIX pricing assumptions, automatically determining the correct Spot product for each AutoScaling group. This enables accurate billing for Windows, RHEL, and SUSE instances while increasing type diversification.

### Multi-Region Flexibility

AutoSpotting can now deploy in any AWS region as its primary installation point, reducing networking costs and eliminating previous Virginia-only restrictions. Regional data for savings reports now stores locally.

### Security Group Fixes

Resolved issues causing security group duplication and incorrect default assignments that previously prevented Spot instances from launching properly.

## Additional Improvements

- Increased InService timeouts to 13 minutes for lifecycle hook scenarios
- Automatic termination of unattached Spot instances after 15 minutes
- Improved event forwarding reliability through EventBridge cross-region support
- CloudFormation stack state handling corrections
- Terraform code enhancements supporting multiple reporting addresses, existing IAM roles, and permissions boundaries

The updated version is available on the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6) for CloudFormation and Terraform deployments.

## Images

![](/images/blog/leanercloud-content/leanercloud-product-updates/image-2.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/leanercloud-product-updates/image-3.png)

![New AutoSpotting releases ](/images/blog/leanercloud-content/leanercloud-product-updates/image-4.png)

![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/leanercloud-product-updates/image-5.png)

