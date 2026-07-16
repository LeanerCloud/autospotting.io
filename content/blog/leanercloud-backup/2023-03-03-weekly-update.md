---
title: "Weekly Update - 3 Mar 2023"
date: 2023-03-03T00:00:00+00:00
description: "Terraform updates, Spot Savings Estimator launch, and website improvements"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "terraform", "savings-estimator", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

## Terraform Updates

The week began with synchronizing the AutoSpotting Terraform module with the CloudFormation template. Previously, the module deployed an outdated October 2021 release containing numerous known issues. Now it rolls out the latest AWS Marketplace version with all recent features exposed.

The module includes enhancements like supporting multiple email addresses for reports (versus CloudFormation's single address limitation). Backend improvements eliminate the need for double Terraform runs for most users—except when deploying to opt-in AWS regions.

Users currently relying on Terraform-based AutoSpotting installations should check the [Terraform Registry](https://registry.terraform.io/modules/AutoSpotting/autospotting/aws/latest) for this release.

## Spot Savings Estimator Launch

A GUI tool underwent release this week as an open-source project. Originally envisioned as proprietary software exclusive to paying customers, feedback from DevOps engineers revealed stronger adoption potential through transparency. This approach widens user reach while maintaining conversion pathways.

The tool functions independently from AutoSpotting while enabling convenient configuration through tags. Early reception proved positive: "882 Clones from 246 Unique cloners and 534 Views from 115 Unique visitors" since announcement.

## Website and Marketplace Updates

The [autospotting.io](http://autospotting.io) website and AWS Marketplace listing received updates highlighting the Savings Estimator as an onboarding mechanism. Related refinements to the AWS Marketplace CLI tool streamlined copywriting processes.

## Forward Focus

Development emphasis shifts toward marketing and sales efforts. Combined with parallel instance replacement features and the Savings Estimator, the offering delivers both cost and time savings for Spot adoption. Community referrals remain welcomed.

## Images

![New AutoSpotting releases ](/images/blog/leanercloud-content/weekly-update-3-mar-2023/image-2.png)

![How we maximized task density on our ECS cluster by avoiding burstable instances](/images/blog/leanercloud-content/weekly-update-3-mar-2023/image-3.png)

![How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD](/images/blog/leanercloud-content/weekly-update-3-mar-2023/image-4.png)

