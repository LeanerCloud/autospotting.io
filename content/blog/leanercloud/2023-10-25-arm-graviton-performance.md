---
title: "How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD"
seo_title: "Why Arm Chips Like AWS Graviton Are Faster and Cheaper"
date: 2023-10-25T00:00:00+00:00
description: "Understanding why Arm-based processors deliver better performance and lower costs than x86 alternatives"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Graviton", "Performance"]
tags: ["aws", "graviton", "arm", "x86", "performance", "cost-optimization", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-10-25-arm-graviton-performance.jpg"
draft: false
---

## The Arm price/performance paradox

In my cost optimization gigs I often see how many people don't understand how Arm chips (such as AWS Graviton and Apple M1/M2) can be both cheaper and faster than x86 chips from Intel and AMD.  
  
It's counter-intuitive, so many think that since Graviton is ~20% cheaper, it must also be slower.  
  
To explain this paradox, imagine two development teams:

- Team x86 has 4 engineers and 6 managers
- Team Arm has 6 engineers and 2 managers

That's how you can get 50% more output at 20% less costs.

## Why so many managers on the x86 team?

To continue the analogy, imagine that they're two full-stack engineering teams.  
  
The x86 team is an offshore agency working for a big corp.

They need to support all sorts of existing projects, from maintaining a high performance backend in C/C++, to web applications in Python and frontends in Javascript, CSS and HTML.  
  
Some of the engineers are great at C/C++, others know Python and others know web technologies.

Each of the team members knows a couple of languages really well, for example the C/C++ guy also knows some Cobol. If there's need for some maintenance work he can cover both, which works great usually, but can become a problem when he's needed at the same time for a big C/C++ project and also some Cobol maintenance work on the company's legacy mainframe.  
  
The customers often don't know exactly what they want, and that’s why the team needs an army of managers to figure out what the customers mean, creating user stories that make sense to developers and making sure engineers are utilized to their maximum potential and everything gets done for the customers.

They're also for whatever reasons split in two different groups, from different time zones. It works well for smaller projects which can be handled entirely in the same country, but they sometimes need to work together, which becomes hard when they have to hand things over to the other group.

## The Arm development team

The Arm team is also a full-stack team, but they standardized on Javascript.

Everyone knows Javascript and can use it both on the backend and frontend. The projects are relatively similar to each other, and many building blocks can be reused across projects.

Also each engineer is dedicated to work on a single project at a given time. This may be wasteful when there's not much work to do, so engineers may spend lots of time on HackerNews, but this focus on a single project helps a lot under high load.

They're also hired in-house as employees and not from an external agency, since their big corp saw the cost benefits or running their own dev team, and they're all sitting in the same room.  
  
They still need some managers, but nowhere near the amount needed by the x86 team.

## Back to the chips

It very similar in the chips world.  
  
x86 has a complex instruction set with a lot of legacy, instructions of various sizes, so it needs a lot of "management" to achieve high instruction level parallelism, which makes chips complex and power hungry.

(I have an M1 Macbook but still remember my previous Intel Macbook with fans always spinning as if about to take off. I rarely hear the fan on my M1, it's blissfully silent).  
  
Arm has fixed size instructions, which allows it to get higher instruction level parallelism with less "management" logic, so it can afford to have more execution units which can get more done while still consuming less power.

They're also custom silicon, build in-house for their specific needs, and avoiding the extra costs of buying off-the-shelf chips.

x86 instances have two physical processors in NUMA configuration, which may cause cause performance issues when you run the biggest instances that span both physical CPUs. They also usually have SMT/Hyperthreading enabled, which helps under light utilization, but introduces a performance penalty under high load.  
  
When you combine all these you can get more bang for less bucks with Arm, be it Graviton on AWS or M1/M2 on Apple devices.

## How to start with Graviton on AWS?

If you want to get started with Arm-based Graviton chips, the lowest hanging fruit is for managed services such as RDS, ElastiCache or OpenSearch, where all you need to do is change the instance type.

Graviton works for many other workloads but it may require some work which may not be trivial in some cases.

## How can I help you adopt Graviton

I used to work at AWS as Specialist Solution Architect for Flexible Compute, focusing on Spot and Graviton. I now help customers optimize their AWS setup for lowest costs and increased performance, and among many other things, I also help them adopt Graviton at scale.

I charge based on the savings I drive, so it's in my interest to save my customers the most money in the shortest possible time.

For accelerating some of this work, I built tooling that automates certain low hanging fruits such as converting EBS volumes to GP3, which I then extended to optimize RDS storage and converting RDS databases to Graviton.

For increased savings it also rightsizes the databases to match the actual needs of the workload based on its CPU and memory metrics.

It's a CLI that looks and feels much like Terraform, with plan and apply modes:

![](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-2.png)

running in plan mode

(It will eventually run continuously and install from the AWS Marketplace like my other tools [AutoSpotting](https://autospotting.io/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-can-arm-chips-like-aws-graviton-or-apple-m1-2-be-faster-and-cheaper-than-x86-chips-from-intel-or-amd) and [EBS Optimizer](https://leanercloud.com/ebs-optimizer?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-can-arm-chips-like-aws-graviton-or-apple-m1-2-be-faster-and-cheaper-than-x86-chips-from-intel-or-amd), but for now it's only available through my services offering as a CLI tool I use to accelerate my work.)

If this sounds like something that may benefit you, drop me a message on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=how-can-arm-chips-like-aws-graviton-or-apple-m1-2-be-faster-and-cheaper-than-x86-chips-from-intel-or-amd) and I'm happy to help.

-Cristian

#### Keep reading

[![New AutoSpotting releases ](/images/blog/leanercloud-content/can-arm-chips-like-aws-graviton-apple-m12-faster-cheaper-x86-chips-intel-amd/image-3.png)

## New AutoSpotting releases

Releasing AutoSpotting 1.3.0(new features) and 1.2.3(important billing bugfix)


## New AutoSpotting release, adding support for Mixed Autoscaling groups

How ECS capacity providers inspired me to fix an ancient AutoSpotting issue


## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


View more
