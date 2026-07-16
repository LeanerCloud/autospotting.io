---
title: "How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD"
date: 2023-10-25T00:00:00+00:00
description: "Understanding why Arm-based processors deliver better performance and lower costs than x86 alternatives"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Graviton", "Performance"]
tags: ["aws", "graviton", "arm", "x86", "performance", "cost-optimization", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-10-25-arm-graviton-performance.png"
draft: false
---

Many people struggle to understand how Arm-based processors can simultaneously deliver better performance and lower costs than x86 alternatives. The counterintuitive nature of this reality leads some to assume that a 20% cost reduction must correlate with slower speeds.

To illustrate this concept, consider two hypothetical engineering teams: Team x86 operates with 4 engineers and 6 managers, while Team Arm functions with 6 engineers and 2 managers. The result? Fifty percent greater output at twenty percent lower expenses.

## Why so many managers on the x86 team?

The x86 team functions as an external agency supporting diverse legacy projects spanning C/C++ backends, Python web applications, and JavaScript frontends. Engineers specialize in different languages, creating coordination challenges. Frequent customer uncertainty necessitates extensive managerial oversight to translate requirements into actionable development tasks and optimize resource allocation.

Their distributed structure across time zones compounds these complexities, making inter-team handoffs particularly problematic.

## The Arm development team

The Arm team standardized on JavaScript across their entire stack. All engineers possess proficiency in this single language, enabling seamless work on both backend and frontend components. Project similarities facilitate code reuse, while dedicated single-project focus maximizes performance during peak loads.

As in-house employees rather than external contractors, the team benefits from physical proximity and reduced coordination overhead, requiring significantly fewer managers.

## Back to the chips

This organizational dynamic mirrors chip architecture principles. x86 processors feature complex instruction sets with substantial legacy baggage and variable instruction sizes, requiring elaborate "management" logic to achieve instruction-level parallelism—resulting in power-hungry, complex designs.

Arm employs fixed-size instructions, enabling higher instruction-level parallelism with minimal overhead. This efficiency permits more execution units operating simultaneously while consuming less power.

Custom silicon design optimized for specific applications further reduces costs compared to off-the-shelf alternatives. x86 instances employ dual-processor NUMA configurations and hyperthreading, which introduce performance penalties under heavy loads. Combined, these factors enable Arm processors to deliver superior performance-per-dollar metrics.

## Getting started with Graviton on AWS

Managed services like RDS, ElastiCache, and OpenSearch represent the lowest-friction adoption path—requiring only instance type modifications.

## Professional adoption support

The author previously served as an AWS Specialist Solution Architect specializing in Flexible Compute and Graviton optimization, now offering AWS cost optimization consulting with performance improvements included. Custom tooling automates conversions of EBS volumes to GP3, RDS storage optimization, and database migration to Graviton, while rightsizing instances according to actual workload metrics.

Interested parties are encouraged to connect via LinkedIn for consultation services.

## Images

![](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-2.png)

![New AutoSpotting releases ](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-3.png)

![New AutoSpotting release, adding support for Mixed Autoscaling groups](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-4.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-5.png)

