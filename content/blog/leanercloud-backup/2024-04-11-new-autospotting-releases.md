---
title: "New AutoSpotting Releases"
date: 2024-04-11T00:00:00+00:00
description: "AutoSpotting 1.3.0 with new features and 1.2.3 with critical billing fix"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "spot-instances", "releases", "leanercloud"]
featured_image: "/images/blog/leanercloud/2024-04-11-new-autospotting-releases.png"
draft: false
---

The AutoSpotting team announced two simultaneous releases: version 1.3.0 with new features and version 1.2.3 with a critical billing fix. After nearly eight months without updates, the releases contain "dozens of relatively small reliability, correctness and performance improvements."

## Key Improvements in 1.3.0

The major release includes significant enhancements:

- **Instance replacement logic** now avoids reduced capacity by attaching new instances before terminating old ones
- **Spot diversification** within Availability Zones can be configured for increased reliability
- **ECS task draining** improved for Spot termination scenarios
- **Deployment awareness** pauses execution during Beanstalk deployments and autoscaling instance refreshes
- **Stateful resource migration** automatically moves EBS volumes and Elastic IPs to replacement instances
- **Capacity rebalancing disabled** to reduce instance churn
- **Unattached instances reaped** to prevent unnecessary costs
- **Launch template management** improved with better lifecycle handling
- **EBS volume conversion** to GP3 for cost savings on smaller volumes
- **Memory consumption reduced** from 450MB to approximately 150MB
- **SDK upgraded** to Go v2 for better performance
- **Security improvements** including gosec static code checks

## Pricing Changes

The company raised fees as of version 1.3.0. The "savings cut will double from 5% to 10%," meaning customers retain 90% of savings instead of 95%. However, older versions remain available indefinitely for those preferring to maintain current deployments.

## Version 1.2.3 Bugfix Details

This release addresses a billing calculation error where "any running Spot instances" were counted regardless of their origin. One customer using alternative Spot solutions reported overcharges, prompting the fix. Terraform deployment billing was also corrected.

## Technical Enhancements

The codebase underwent modernization including Go version updates, converted to SDK for Go v2, and reduced memory footprint. The team fixed approximately a dozen crash scenarios and improved error handling throughout the system.

## Images

![](/images/blog/leanercloud-content/new-autospotting-releases/image-2.png)

![](/images/blog/leanercloud-content/new-autospotting-releases/image-3.png)

![How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD](/images/blog/leanercloud-content/new-autospotting-releases/image-4.png)

![How we maximized task density on our ECS cluster by avoiding burstable instances](/images/blog/leanercloud-content/new-autospotting-releases/image-5.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/new-autospotting-releases/image-6.png)

