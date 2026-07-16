---
title: "Releasing Additional Terraform Building Blocks to the LeanerCloud ONCE Bundle"
date: 2024-06-02T00:00:00+00:00
description: "Terraform serverless microservice modules added to LeanerCloud ONCE bundle"
author: "Cristian Magherusan-Stanciu"
categories: ["LeanerCloud", "Terraform", "Tools"]
tags: ["leanercloud", "terraform", "serverless", "aws", "lambda", "ecs", "once-model"]
featured_image: "/images/blog/leanercloud/2024-06-02-terraform-building-blocks.png"
draft: false
---

LeanerCloud has announced an expansion of its ONCE bundle, which now includes Terraform serverless microservice building blocks alongside previously released tools.

## What's Included

The bundle combines new Terraform modules with the existing Optimizer CLI FinOps tool. The latter handles "GP2 to GP3 conversion for EC2 and EDS, and rightsizing" for RDS and related services.

## Key Features of the Terraform Blocks

The new modules provide example implementations for serverless applications using Lambda and ECS Fargate. They feature a layered architecture where each layer publishes values to SSM parameters, enabling higher-level components to consume them without additional configuration.

**Highlights include:**

- Lambda deployments with Function URLs behind CloudFront, ACM certificates, and Route53 DNS
- Shared ECS clusters with load balancers using TLS and wildcard DNS records
- Independent ECS services with name-based virtual hosting
- RDS Aurora Serverless v2 integration with automatic secret injection
- GitHub Actions CI/CD configuration with IAM authentication support

## Licensing and Pricing

Unlike the CLI tools, these building blocks allow proprietary product development and team use. The increased complexity warrants a price adjustment: the bundle now costs $799 (Stripe) and $999 (AWS Marketplace), up $300 from previous pricing.

Perpetual access can be purchased immediately, though the link will expire after the next price increase announcement.

## Images

![](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-2.png)

![My thoughts on the current state of EC2 Spot pricing](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-3.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-4.png)

![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-5.png)

