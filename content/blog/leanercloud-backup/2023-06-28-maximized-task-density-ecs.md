---
title: "How we maximized task density on our ECS cluster by avoiding burstable instances"
date: 2023-06-28T00:00:00+00:00
description: "Overcoming ECS task density limitations caused by ENI exhaustion on burstable instances"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "ECS", "Optimization"]
tags: ["aws", "ecs", "ec2", "networking", "leanercloud", "task-density"]
featured_image: "/images/blog/leanercloud/2023-06-28-maximized-task-density-ecs.png"
draft: false
---

This technical article discusses AWS ECS optimization challenges encountered while migrating an AI startup's infrastructure from Docker-compose instances to ECS. The primary focus centers on task density limitations when using burstable instance types.

## Key Problem

The team initially selected t3a.2xlarge instances for their ECS cluster due to low CPU utilization patterns. However, they encountered an unexpected constraint: despite having available vCPUs and memory, each instance could only run 3 ECS tasks. The limitation stemmed from Elastic Network Interface (ENI) exhaustion, a relatively obscure constraint in ECS documentation.

## Root Cause: ENI Limitations

When using `awsvpc` networking mode—which provides dedicated IPs to tasks and enables security group configuration—each ECS task consumes one ENI. Instance types have fixed ENI quotas. As the article notes, "the scheduler will consider the instance busy when the ENIs are all exhausted," restricting task placement regardless of CPU or memory availability.

## NAT Gateway Discovery

The post reveals an additional unexpected requirement: tasks in `awsvpc` mode need NAT Gateways for internet access even when instances reside in public subnets. This represents a significant hidden cost consideration.

## Solution: ENI Trunking

AWS offers ENI trunking to increase task density while maintaining `awsvpc` benefits. The fix requires a single CLI command:

```bash
aws ecs put-account-setting-default \
  --name awsvpcTrunking \
  --value enabled
```

## Critical Limitation

Burstable instance families (like t3a) don't support ENI trunking. The team switched to m6a.2xlarge—slightly more expensive but capable of running substantially more tasks. This shift enabled better cluster utilization despite higher per-instance costs.

## Key Takeaway

Infrastructure optimization requires understanding AWS service constraints beyond basic resource allocation metrics.

## Images

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-2.png)

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-3.png)

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-4.png)

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-5.png)

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-6.png)

![](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-7.png)

![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-8.png)

![New AutoSpotting releases ](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-9.png)

![New AutoSpotting bugfix release - 1.3.1](/images/blog/leanercloud-content/maximized-task-density-ecs-cluster-avoiding-burstable-instances/image-10.jpeg)

