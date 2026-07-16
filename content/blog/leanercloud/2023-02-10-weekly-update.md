---
title: "Weekly Update - 10 Feb 2023"
date: 2023-02-10T00:00:00+00:00
description: "LeanerCloud GUI development progress and outreach to AutoSpotting OSS users"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "AutoSpotting"]
tags: ["weekly-update", "autospotting", "gui", "community", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

Hello, and welcome to another weekly update.

(I forgot to publish this last week here on beehiiv, so I'm now publishing it together with the current week's update. The current week's update will be scheduled 12h later, in order to accommodate beehiiv restrictions, sorry about that.)

### LeanerCloud GUI

This week I continued the development of the new LeanerCloud GUI.

I made a lot of progress, the GUI is now able to estimate the savings, updating the estimates based on the selected ASGs and their configuration, and also can write a few of the commonly used tags, as well as reading the values set in the previous runs.

There are still a few cosmetic glitches to sort out, but the tagging works pretty well, and I can now start showing this to prospects in my demo sessions and even using it for mass AutoSpotting rollouts at customers or giving it to people to try it out, let me know if you're interested.

You can see the way it looks so far in this Loom video, although there was a bug in AutoSpotting that I uncovered at the end of the recording.

[Loom | Free Screen & Video Recording Software

Use Loom to record quick videos of your screen and cam. Explain anything clearly and easily – and skip the meeting. An essential tool for hybrid workplaces.

www.loom.com/share/cfc08ec635b743da99769b5a62c5f482

![](/images/blog/leanercloud-content/weekly-update-10-feb-2023/image-2.png)](https://www.loom.com/share/cfc08ec635b743da99769b5a62c5f482?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-10-feb-2023)

I'll keep improving this over the next few weeks, stay tuned for more updates. The plan is to add EBS Optimizer support, and further polish the AutoSpotting view, by adding support for more of the commonly used configuration flags, considering RIs, only showing the regions where the user has ASGs, or maybe even adding all ASGs across regions.

### Outreach to AutoSpotting OSS users

For the rest of the week I resumed my outreach campaign of connecting to AutoSpotting users.

I spent a lot of time manually reaching out to a few dozens people who forked it in the past and asking for feedback, but then I realized this should be automated as it was too time consuming and only covered like 10% of them in an entire day.

Today I spent most of my day automating this work with a lot of help from ChatGPT. After a few hours of fiddling I eventually managed to write a script that collects contact information of the fork authors, which I'll use to reach out to them asking for feedback on AutoSpotting and whether they still use it. The goal is to get some testimonials for my website, and maybe some ideas for further improvements.

I plan to extend the same script to people who raised Github issues or watched the repo, and then do the same for the Terraform AutoSpotting repo.

### Talking to people

This week I kept talking to people and again got a few new insights on what kind of things to build next, how to improve my sales and marketing, potential partnerships to pursue or stop pursuing, and also on the product side how to improve AutoSpotting to make it more appealing to Enterprise customers, besides the GUI work I've been doing.

Here are a few of the things I've been hearing from people and/or considering to build in AutoSpotting, besides the things I mentioned above for the GUI:

- adding support for automatically updating the instance type information and/or using attribute based instance type selection
- considering Reserved Instances and Savings Plan coverage.
- instance replacements with failover to multiple OnDemand instance types
- taking over Mixed Instances ASGs

Let me know if there's anything you'd like to see, as besides the GUI I'll also be working on those over the next few weeks, stay tuned for further updates.

That's pretty much it for now,

See you again next week!

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/weekly-update-10-feb-2023/image-3.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks


## How can Arm chips like AWS Graviton or Apple M1/2 be faster and cheaper than x86 chips from Intel or AMD


View more
