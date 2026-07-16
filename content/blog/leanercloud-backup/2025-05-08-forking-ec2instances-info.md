---
title: "Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info"
date: 2025-05-08T00:00:00+00:00
description: "Creating a community-driven fork of ec2instances.info after Vantage development stalled"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["ec2instances", "cloud-instances", "aws", "open-source", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-08-forking-ec2instances-info.png"
draft: false
---

## About ec2instances.info

The ec2instances.info website (also known as instances.vantage.sh) serves as a convenient tool for AWS users to compare EC2 instance types across families based on specifications and pricing. It functions as an online spreadsheet with comprehensive cloud instance data and filtering capabilities.

## The ec2instances.info Open Source Project

Launched in 2011 by Garret "powdahound" Heaton, this project evolved into more than just a UI tool. It includes a comprehensive database of instance type specs and pricing for multiple AWS services including EC2, RDS, and ElastiCache—often more accessible than official AWS APIs.

The author began using this database in 2016 when developing AutoSpotting, eventually becoming a co-maintainer. The project initially relied on HTML table scraping, making maintenance challenging despite its popularity within the AWS community.

## Vantage Acquisition

In 2021, Heaton sold the website to Vantage, a rising FinOps company. The Vantage CEO reassured the author they would maintain it as open source. Engineer Everett Berry subsequently made significant improvements, including a redesign and additional data fields.

## Development Stagnation

After Everett left Vantage approximately one year ago, development effectively ceased. Though the author contributed bug fixes and features (Valkey support, SQL Server pricing), responsiveness declined, with the last CTO commits appearing in October 2024.

## The Fork Initiative

Vantage announced plans to rewrite the website from scratch with low-cost freelancers. The author offered a retainer but was rejected as too expensive. Concerned about project continuity and their dependency on the database for various tools, they created a fork.

### Current Progress

Within days, the fork achieved:

- New GitHub repository at LeanerCloud/cloud-instances.info with issues enabled
- Automated CI/CD builds to CloudFlare R2
- Code cleanup removing Vantage branding
- Community Slack workspace
- New website hosted on CloudFlare at cloud-instances.info

### Future Goals

The fork aims to restore vendor neutrality and community-driven development. Plans include rewriting Azure support code and expanding coverage to additional cloud providers while maintaining AWS data richness. Community contributions are welcome via GitHub or Slack.

## Images

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-2.png)

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-3.png)

![](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-4.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-5.png)

![Vantage just updated ec2instances.info and released all their code, now what?](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-6.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info/image-7.png)

