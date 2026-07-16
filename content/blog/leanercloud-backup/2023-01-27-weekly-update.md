---
title: "Weekly Update - 27 Jan 2023"
date: 2023-01-27T00:00:00+00:00
description: "LeanerCloud GUI development begins, website simplification, and infrastructure improvements"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates"]
tags: ["weekly-update", "gui", "infrastructure", "podcast", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

## LeanerCloud GUI Work

Development began on a new graphical interface tool designed to simplify adoption for new users. The current implementation reads all Auto Scaling Groups from the AWS region and generates a data table displaying ASG name and size information.

Future enhancements planned include additional data fields such as instance types, capacity ranges, and hourly pricing. The interface will become interactive, allowing users to set conversion percentages for Spot instances and view estimated cost savings. Integration with AutoSpotting tags is also planned, plus potential expansion to EBS Optimizer and multi-account AWS organization support.

## Website Updates

The author simplified both leanercloud.com and autospotting.io after feedback indicated information overload. Following a manual configuration error that took autospotting.io offline for several days, the AutoSpotting technical details were reorganized into a dedicated subpage, resulting in cleaner primary website navigation.

## Infrastructure Improvements

The hosting infrastructure and DNS configuration migrated to Terraform-based management, replacing manual setup. This enabled code reuse across both websites and consolidated email services—consolidating Amazon Workmail and Google Workspaces into a single Google Workspaces instance. ChatGPT assisted significantly during Terraform development.

## Podcast News

The LeanerCloud podcast gained traction, reaching 54th place in Israel's Technology category. A recent guest appearance on Jon Myer's podcast covered cost optimization, ChatGPT applications, AWS certifications, community engagement, and audience building.

## Karpenter Support

Conversations about Kubernetes cluster autoscaling led to hands-on Karpenter collaboration. The author seeks to expand container-focused offerings, particularly around Karpenter, since existing tools focus primarily on EC2 Auto Scaling Groups.

## Images

![No alt text provided for this image](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-2.png)

![AutoSpotting comparison matrix](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-3.png)

![Why I recommended ECS instead of Kubernetes to my latest customer](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-4.png)

![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-5.png)

