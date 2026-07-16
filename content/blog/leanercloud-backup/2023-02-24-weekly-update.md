---
title: "Weekly Update - 24 Feb 2023"
date: 2023-02-24T00:00:00+00:00
description: "Customer onboarding challenges, parallel instance replacement breakthrough, and marketplace automation"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "marketplace", "performance", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

This week's update covers several significant developments in AutoSpotting and related tooling, focusing on customer support challenges, parallel instance replacement features, and marketplace automation improvements.

## Customer Onboarding Experience

The author spent considerable time supporting a new customer through onboarding. The customer initially requested an older AutoSpotting version due to their blue-green deployment strategy. While the support experience was praised, the legacy version proved problematic, causing outages due to its "lowest cost Spot allocation strategy" and the customer's limited availability zone configuration. This led to commitments to enhance the latest version for their specific use case.

## Security and Docker Challenges

Docker image scanning flagged security vulnerabilities in Alpine-based images. The irony: AutoSpotting is a static binary with minimal attack surface, yet requires Alpine base layers to pass marketplace scanners that reject "FROM scratch" images. This created unnecessary friction during customer onboarding.

## Parallel Instance Replacements

A major breakthrough emerged from customer feedback about blue-green deployments. The new version now handles instance replacements concurrently rather than sequentially, reducing deployment time from minutes to "10-20 seconds" for typical replacements. This minimizes unwanted OnDemand instance runtime and associated software licensing costs.

## AWS Marketplace Tooling

Frustrated with the marketplace's clunky GUI, the author developed automation tools:

- Created [aws-marketplace-cli](https://github.com/LeanerCloud/aws-marketplace-cli) to streamline release management
- Reduced manual, error-prone processes to automated workflows
- Enabled releases in approximately 5 minutes versus hours of manual effort

## Additional Improvements

- Updated instance type information via ec2-instances-info dependencies
- Added bulk EBS volume tagging support to awstaghelper, completing tagging automation
- Prepared new AutoSpotting marketplace releases incorporating these enhancements

## Looking Ahead

Next week's focus: GUI development for easier configuration and tagging, followed by marketing and sales initiatives.

## Images

![Why I just declined a job offer from a billionaire friend](/images/blog/leanercloud-content/weekly-update-24-feb-2023/image-2.png)

![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/weekly-update-24-feb-2023/image-3.png)

![Progress report for the first week after forking ec2instances.info](/images/blog/leanercloud-content/weekly-update-24-feb-2023/image-4.png)

