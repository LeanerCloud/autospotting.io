---
title: "Progress Report for the First Week After Forking ec2instances.info"
date: 2025-05-14T00:00:00+00:00
description: "First week of development on cloud-instances.info fork: domains, hosting, and community response"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["cloud-instances", "ec2instances", "aws", "cloudflare", "terraform", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-14-cloud-instances-progress-report.png"
draft: false
---

The author forked the AWS instance comparison site as a vendor-neutral alternative at cloud-instances.info. This report covers the first week's activities, decisions, and technical challenges.

## The Domain

Domain selection prioritized being generic, provider-agnostic, and affordable. The criteria excluded ties to specific cloud vendors or FinOps tools. The chosen domain, cloud-instances.info, met all requirements and cost only "$3.16 for the first year" on Namecheap.

## The Git Repository

The fork lives under the LeanerCloud GitHub organization, potentially moving later with multiple co-maintainers. Initial build attempts failed because Vantage likely maintains a private fork containing build fixes and Azure support code unavailable in the public version. Several commits fixed compilation issues.

## Website Hosting Challenges

### CloudFlare Pages Limitations

CloudFlare Pages imposed a 25MB file size limit, insufficient for large instance data JSON files approaching 100MB. This necessitated using CloudFlare R2 object storage instead.

### R2 Technical Issues

R2 couldn't configure an `/index.html` file for the root directory. The workaround involved creating "a simple CloudFlare worker" that redirects from `/` to `/index.html` while preserving query parameters. This introduced fugly URLs requiring `/index.html` in paths.

A JavaScript bug emerged when switching regions—links became malformed as `/index.htmlregional-JSON-file.json`. This was quickly identified and fixed in the codebase.

### Terraform Provider Problems

The CloudFlare Terraform provider faced significant issues. Version 5.4.0 lacked R2 bucket support, while 5.5.0 failed to compile. Earlier versions supporting buckets broke when creating CloudFlare Workers. Community advice suggested mixing provider versions—using an older version for bucket creation and current versions for other resources—which ultimately resolved the infrastructure-as-code setup.

## Community Response

The announcement received strong reception. People committed to updating bookmarks. A BMW FinOps engineer contributed code improvements within days. In contrast, AutoSpotting took 2-3 months to gain initial users and 5 months for first pull requests.

## Vantage Communication

Vantage acknowledged the fork as the author's legitimate right, though remained unsupportive. The previous communication gap stemmed from a contact having a premature newborn without setting an auto-responder. The author proposed upstream collaboration, which Vantage hasn't addressed.

## Sponsorship Opportunities

Multiple FinOps vendors offered development sponsorships and FinOpsX conference attendance. The author seeks to monetize while maintaining vendor neutrality and open development, hoping to replicate AutoSpotting's original vision.

## Technical Progress

Work included dozens of commits addressing builds, removing Vantage marketing content, securing the infrastructure, and enabling security features (linters, code analysis, dependency checks). Pull request ephemeral environments are in progress, though bucket cleanup still needs refinement.

## Next Steps

Priorities include completing ephemeral environments, addressing security scanner issues, implementing Azure support, and encouraging community contributions to existing GitHub issues.

## Images

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-2.png)

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-3.png)

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-4.png)

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-5.png)

![My thoughts on the current state of EC2 Spot pricing](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-6.png)

![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-7.png)

![Why I recommended ECS instead of Kubernetes to my latest customer](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-8.png)

