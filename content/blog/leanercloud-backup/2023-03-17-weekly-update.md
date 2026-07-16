---
title: "Weekly Update - 17 Mar 2023"
date: 2023-03-17T00:00:00+00:00
description: "AutoSpotting architecture improvements driven by large enterprise customer onboarding"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "aws", "architecture", "eventbridge", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

This week focused heavily on AutoSpotting development, driven by onboarding a large enterprise customer whose proof-of-concept alone exceeds all current customers combined. The customer's restrictive Landing Zone configuration blocked IAM role creation and VPC provisioning, forcing architectural improvements.

## Current Architecture Overview

AutoSpotting operates as a serverless, event-driven application primarily built in Lambda with Go. The system has evolved significantly since 2016:

- **Event Processing:** Responds to instance launches, Spot terminations, and lifecycle hooks
- **Regional Distribution:** Cross-region event forwarding through Lambda functions invoking the main Lambda
- **Queue Management:** FIFO SQS queues serialize events to prevent ASG API conflicts
- **Billing Infrastructure:** Fargate cron job (hourly) handles AWS Marketplace billing; requires VPC and ECR registry
- **State Storage:** SSM parameter store tracks savings; SNS topics send daily reports

## Key Improvements Implemented

The author made the Terraform code optionally accept existing IAM roles, subnets, and permission boundaries. EventBridge rules replaced regional Lambda forwarders, simplifying cross-region event routing. Public ECR repository support enables trial deployments before AWS Marketplace purchase.

## PoC Progress

Initial testing revealed Windows instances exceeded Lambda's one-minute timeout before achieving InService status. Timeout increases resolved this issue; future improvements will add InService event rules to eliminate internal waiting.

## Presentation Activities

The author delivered an AutoSpotting talk at a partner company, demonstrating the Savings Estimator tool and configuration workflows. The presentation was well-received and reportedly recorded.

## Images

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-2.png)

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-3.png)

![](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-4.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-5.png)

![New AutoSpotting releases ](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-6.png)

![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/weekly-update-17-mar-2023/image-7.png)

