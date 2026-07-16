---
title: "Vantage Just Updated ec2instances.info and Released All Their Code, Now What?"
date: 2025-05-26T00:00:00+00:00
description: "Analyzing Vantage's refreshed ec2instances.info and planning the future of cloud-instances.info fork"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["ec2instances", "cloud-instances", "vantage", "open-source", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-26-vantage-code-release.png"
draft: false
---

Vantage has released a significantly refreshed version of ec2instances.info with modernized frontend code and previously private infrastructure components now open-sourced. The author, who recently created cloud-instances.info as a vendor-neutral fork, examines what changed and considers the path forward.

## Why the Fork Happened

The original public codebase hadn't received meaningful updates for six months. The author suspected Vantage maintained a private branch containing marketing improvements, scraper fixes, and Azure functionality that weren't available publicly. When the author requested clarification about the rewrite's direction and data source compatibility, "they weren't able to answer," prompting the decision to fork as a safeguard.

## What Vantage Released

**Positive Changes:**
- Completely rebuilt frontend using React for faster data rendering
- Removed the invasive marketing banner at the top (replaced with discrete "presented by Vantage" attribution)
- Azure support now positioned prominently at the top
- Migrated hosting to Cloudflare Workers and R2
- Published previously private Azure code and GitHub Actions
- Released infrastructure-as-code documentation

**Issues Identified:**
- CPU and memory filters are broken
- Price sorting displays unavailable instance types first (problematic in regions with limited availability)
- Region search field removed from dropdown, making navigation harder

## Looking Forward

The author plans to maintain the fork as a vendor-neutral alternative "much like we have the Chromium and VSCodium projects." The immediate strategy involves keeping the older, stable version until Vantage resolves UI/UX regressions, then gradually rebasing improvements and contributing useful changes back upstream.

The author acknowledges some sponsorship complications from this situation but remains committed to sustainable open-source development.

## Images

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-2.png)

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-3.png)

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-4.png)

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-5.png)

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-6.png)

![](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-7.png)

![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-8.png)

![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-9.png)

![Why I just declined a job offer from a billionaire friend](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-10.png)

