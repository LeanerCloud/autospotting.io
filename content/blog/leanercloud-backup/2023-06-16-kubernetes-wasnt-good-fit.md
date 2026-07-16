---
title: "Why Kubernetes Wasn't a Good Fit for Us"
date: 2023-06-16T00:00:00+00:00
description: "Understanding when Kubernetes complexity outweighs its benefits for small teams"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Kubernetes", "ECS"]
tags: ["kubernetes", "ecs", "aws", "infrastructure", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-06-16-kubernetes-wasnt-good-fit.png"
draft: false
---

The author explains why his team chose Amazon ECS over Kubernetes for a recent customer project, addressing feedback from a previous viral post that generated over 20,000 views.

## Key Points

### Kubernetes Architecture Basics

Kubernetes consists of a control plane managing internal services and worker nodes running applications. Cloud providers like AWS manage the control plane through services such as EKS, handling encryption, API security, and patches. Controllers integrate applications with cloud infrastructure, managing load balancers, storage volumes, and DNS records.

### The Elegance vs. Reality Problem

While Kubernetes offers developer-friendly syntax—such as annotations that automatically provision load balancers and DNS records—this convenience masks underlying complexity. The controllers powering these features require installation and maintenance by platform engineers.

### The Cost of Overhead

At small scale, the infrastructure complexity becomes disproportionate. Setting up AWS Load Balancer Controller and ExternalDNS involves multiple Helm commands and configuration steps. The author argues this resembles "building a factory that makes only a handful of products."

### ECS as an Alternative

Using Terraform to directly provision load balancers and DNS records, while more verbose, provides immediate validation feedback. Developers catch configuration errors before deployment rather than debugging controller logs later.

### The Bottom Line

Kubernetes makes sense at scale with dedicated platform teams. For small organizations lacking operational expertise, ECS represents a pragmatic starting point that scales with actual needs rather than imposing fixed complexity overhead.

## Images

![](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-2.png)

![](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-3.png)

![Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks ](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-4.png)

![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-5.png)

![Progress report for the first week after forking ec2instances.info](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-6.png)

