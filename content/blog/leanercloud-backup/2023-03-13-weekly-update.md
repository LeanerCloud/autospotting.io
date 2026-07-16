---
title: "Weekly Update - 13 Mar 2023"
date: 2023-03-13T00:00:00+00:00
description: "Sales initiatives, GitHub maintenance, infrastructure optimization, and customer onboarding progress"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "sales", "github", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

This weekly progress report covers sales initiatives, open source releases, GitHub maintenance, infrastructure optimization, and significant customer onboarding work for AutoSpotting.

## Sales and Marketing

The author shifted focus toward business development after completing an intensive development sprint. Outreach efforts targeted former AutoSpotting users and decision-makers at companies that could benefit from the tools. Several collaboration opportunities emerged from these conversations.

## GitHub Issue Triage and Dependabot Setup

Community Edition issues were systematically reviewed and organized. Approximately one-third of open issues were resolved, reducing the total from 45 to 30. Dependabot was configured to manage dependency updates automatically, addressing potential security vulnerabilities. The private AWS Marketplace fork remained current, though the open source version required updates and rebase work.

## Infrastructure Cost Optimization

AWS test environment expenses increased due to extensive instance testing during AutoSpotting and Savings Estimator development. The author converted testing infrastructure to utilize T4g.small instances available through a free tier promotion. AWS Config charges were reduced through careful configuration within the ControlTower setup.

## Open Source Releases

Three consecutive weeks of releases occurred: the Terraform email automation module, AWS Marketplace CLI, and Savings Estimator. These tools aim to benefit the broader developer community while generating interest in commercial offerings. Most software remains open source except for specific monetization-critical features.

## Customer Onboarding

A significant prospect required assistance overcoming IAM permission restrictions in their restricted Landing Zone environment. This engagement prompted improvements to AutoSpotting's Terraform infrastructure, including EventBridge support and enhanced flexibility for restricted environments.

## Infrastructure Enhancements

Terraform code received substantial improvements enabling deployment within restricted environments. EventBridge replaced Lambda functions for cross-region event forwarding. Ongoing work addresses parallel instance replacement limitations and Windows billing optimization.

## Recognition and Partnerships

The author received positive testimonials from new customers, including former AWS employees. An additional collaboration with another ex-Amazon engineer was announced to advance EBS Optimizer development, allowing focus to concentrate on AutoSpotting expansion.

## Upcoming Activities

Plans include continued sales and marketing efforts, customer onboarding support, and a company presentation in Berlin.

## Images

![](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-2.png)

![Progress report for the first week after forking ec2instances.info](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-3.png)

![Vantage just updated ec2instances.info and released all their code, now what?](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-4.png)

![Why I just declined a job offer from a billionaire friend](/images/blog/leanercloud-content/weekly-update-13-mar-2023/image-5.png)

