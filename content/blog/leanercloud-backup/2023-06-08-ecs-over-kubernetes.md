---
title: "Why I recommended ECS instead of Kubernetes to my latest customer"
date: 2023-06-08T00:00:00+00:00
description: "Choosing ECS for cost optimization and simplicity at an AI startup"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "ECS", "Kubernetes", "Case Study"]
tags: ["aws", "ecs", "kubernetes", "cost-optimization", "docker", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-06-08-ecs-over-kubernetes.png"
draft: false
---

The author shares his experience optimizing AWS costs for an AI startup, detailing why he recommended ECS over Kubernetes and how this decision led to broader infrastructure modernization.

## Initial State of Affairs

The startup's CTO sought help extending their AWS credits runway. Initial analysis revealed:

- **EC2** consumed over half their budget (expected for AI/ML workloads)
- **EBS volumes** represented significant spend, with many attached to stopped instances
- About half of EBS volumes were outdated GP2 types convertible to GP3
- **Amazon MQ** was oversized and underutilized
- **RDS** ran large instances with minimal usage
- Applications used docker-compose on EC2 instances, checking out code and building containers locally

The approach created "pets" rather than "cattle"—each instance had custom configurations, making scaling inefficient and creating security risks by storing source code and secrets on production instances.

## Proposals and Low-Hanging Fruit

The team began with quick wins: converting EBS volumes to GP3 and reducing IO2 IOPS provisioning. These actions alone would "save them yearly a few times more than all my consultancy fees," the author notes.

## Why ECS Instead of Kubernetes?

Rather than implement a complex Kubernetes setup via EKS, the author recommended ECS because:

1. **Team capability**: The startup lacked DevOps expertise; EKS would have been overwhelming
2. **Cost efficiency**: ECS avoids Kubernetes control plane costs
3. **Familiar migration path**: Converting docker-compose to ECS was less disruptive than full Kubernetes adoption
4. **Infrastructure-as-Code alignment**: The choice complemented their Terraform and GitHub Actions implementation
5. **GPU support**: ECS on EC2 provided better flexibility for GPU workloads than Fargate

## Implementation Details

The team created a Terraform-based solution that:

- Builds Docker images and pushes them to ECR automatically
- Eliminates the need for source code and secrets on running instances
- Provides built-in container logging and metrics
- Enables right-sizing individual services based on actual consumption
- Supports eventual migration to Spot instances for additional savings

A key challenge emerged when containers using fixed Prometheus ports conflicted in bridge networking mode. The solution involved switching to "awsvpc" networking, which provides better isolation and scalability.

## Next Steps

The plan includes:
- Converting remaining microservices to ECS
- Right-sizing tasks based on metrics
- Implementing Spot instances using AutoSpotting for 50-60% EC2 savings
- Purchasing Savings Plans or Reserved Instances for remaining On-Demand capacity
- Downsizing and optimizing the RDS database

## Outcome

The author expects a "5x return on my fee plus their time/effort investment in the first year," without accounting for improved deployment pipelines and enhanced security posture from keeping secrets off production instances.

## Images

![](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-2.png)

![](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-3.png)

![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-4.png)

![Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks ](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-5.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-6.png)

