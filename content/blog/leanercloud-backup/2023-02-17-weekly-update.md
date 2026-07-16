---
title: "Weekly Update - 17 Feb 2023"
date: 2023-02-17T00:00:00+00:00
description: "Re-launching AutoSpotting OSS, instance type data improvements, and Reserved Instances awareness"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "open-source", "community-edition", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

This week focused on development work aimed at reducing friction for new users and improving the experience for existing users. Despite a shortened week due to personal circumstances, significant progress was made across multiple initiatives.

## Re-launching AutoSpotting OSS as Community Edition

After fixing a bug in AutoSpotting, the author reconsidered the project's direction. Previously, a warning stated the public code was no longer maintained, directing users to the commercial offering instead.

The approach has shifted. "AutoSpotting is an Open Core product," with bug fixes and external contributions now accepted for the Community Edition. Major new features remain reserved for the commercial version, ensuring business viability while supporting the open-source community.

## Instance Type Data Updates

AutoSpotting requires current CPU, memory, and pricing data to function optimally. Previously, users needing newer instance types had to update the entire application. The author began implementing automatic data refresh capabilities using the [ec2-instances-info](https://github.com/LeanerCloud/ec2-instances-info) library.

With ChatGPT assistance, a functional prototype was completed in under two hours, enabling runtime data updates without full application rebuilds.

## Instance Type Data API

To gain independence from external hosting and provide flexibility, a new Lambda-based API was developed to serve instance data. The system uses CloudFront, Route53, and DynamoDB for authentication via API keys.

## ec2instances.info Contributions

The author identified and fixed upstream data parsing issues, contributing improvements to the build system as well. This collaboration opened discussions with Vantage about enhancing instance data infrastructure.

## Reserved Instances and Savings Plans Awareness

Development began on detecting Reserved Instance and Savings Plan coverage to prevent AutoSpotting from inadvertently replacing covered capacity. A Twitch stream documented the progress, though network limitations caused video quality issues.

## OSS Mass Tagging

The [awstaghelper](https://github.com/mpostument/awstaghelper) tool was enhanced to support AutoScaling Groups, enabling users to mass-tag resources via CSV without proprietary tools.

## Customer Support

A new customer required an older AutoSpotting version due to deployment concerns. While initial results were positive, increased Spot interruptions forced a temporary shutdown. The issue may resolve once the customer utilizes all availability zones.

## Plans Forward

Priority shifts to addressing customer deployment style concerns, potentially enabling parallel instance replacements. Following that resolution, work resumes on the GUI release and Reserved Instance awareness features.

## Images

![Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info](/images/blog/leanercloud-content/weekly-update-17-feb-2023/image-2.png)

![New AutoSpotting bugfix release - 1.3.1](/images/blog/leanercloud-content/weekly-update-17-feb-2023/image-3.jpeg)

![How we maximized task density on our ECS cluster by avoiding burstable instances](/images/blog/leanercloud-content/weekly-update-17-feb-2023/image-4.png)

