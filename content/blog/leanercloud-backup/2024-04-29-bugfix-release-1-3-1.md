---
title: "New AutoSpotting Bugfix Release - 1.3.1"
date: 2024-04-29T00:00:00+00:00
description: "Critical fixes for Default VPC placement and Lambda timeout issues"
author: "Cristian Magherusan-Stanciu"
categories: ["AutoSpotting", "Releases"]
tags: ["autospotting", "aws", "bugfix", "releases", "leanercloud"]
featured_image: "/images/blog/leanercloud/2024-04-29-bugfix-release-1-3-1.jpg"
draft: false
---

This release addresses two regressions found in version 1.3.0. The author notes this update is "strongly recommended to users that don't use Default VPCs for EC2 instances."

## Issues Fixed

### 1. Default VPC Instance Placement Problem

The first regression involved instance launch failures outside the Default VPC. Previously, the tool used Subnet IDs for instance placement, which worked reliably. However, when preparing 1.3.0 to support EC2 Classic configurations, the code was switched to use Availability Zone names instead.

This change inadvertently caused all instances to launch in the Default VPC for most users. The author's test environment used a Default VPC, masking the issue until customers reported failures.

The fix reverts to Subnet ID-based placement for standard configurations while maintaining Availability Zone naming for EC2 Classic setups.

### 2. Lambda Timeout During Spot Instance Replacement

The second regression occurred when AutoSpotting waited for terminating Spot instances to reach Running state—a problematic scenario when grace periods exceeded the two-minute Spot termination notice window.

Since instances were already terminated, the waiting logic never completed, causing Lambda timeouts and suspended AutoScaling Group processes. This primarily affected configurations using a single instance type with the `current` keyword.

## Update Instructions

Users should deploy version 1.3.1-0 or newer using the Docker image tag configuration available on AWS Marketplace.

## Images

![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/new-autospotting-bugfix-release-131/image-2.png)

![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/new-autospotting-bugfix-release-131/image-3.png)

![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/new-autospotting-bugfix-release-131/image-4.png)

