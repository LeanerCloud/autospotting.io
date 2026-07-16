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

Hello, and welcome to this week's update.

Without further ado, let's dive right in!

## LeanerCloud GUI work

Like I mentioned last week, I started the week by working for a couple of days on the new LeanerCloud GUI tool, that would hopefully remove some of the friction of adopting my tools for new users.

I now have code that reads all ASGs from the current region and generates a little table based on it.

Here's how it looks so far, with real data coming from my test AWS account populated in the ASG Name and Size fields:

![No alt text provided for this image](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-2.png)

Over the next weeks I'll work on things like populating the table with more data fields, maybe give a list of instance types, min/max capacity, current hourly price, etc.

Then the plan is making it interactive, like being able to use the value in the Convert to Spot checkbox, but also to set the number or percentage of instances to convert to Spot, and then based on those to estimate savings for each ASG in a new field, and to apply the tags expected by AutoSpotting for that configuration.

And once AutoSpotting is done, to do the same for EBS Optimizer, and maybe even extend everything to work across any number of accounts in an AWS organization, from a single pane of glass.

I'd love to see your feedback on what else y'all want to see in this GUI tool.

## Website updates

For the rest of the week I spent some time improving my web presence, to simplify and clarify it further.

Based on feedback from people I talked a few weeks back, the website was too heavy in information, and then I somehow also broke [autospotting.io](http://autospotting.io/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-27-jan-2023) through some manual changes and it was down for a couple of days, and I only realized after someone complained about it on Reddit.

So I finally spent on time to fix it and carved out the information on AutoSpotting from [LeanerCloud.com](https://leanercloud.com) and used it to update [AutoSpotting.io](https://autospotting.io).

I then also moved the EBS Optimizer information into a sub-page, so [leanercloud.com](http://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-27-jan-2023) has much less technical details about my tools.

I'm not done with it, but so far I'm happy with this simplified structure and will evolve it going forward.

I'd really appreciate if you could also have a look and let me know what you think I should improve about them by leaving a comment below.

## Infrastructure improvements

While at it, I also spent some time improving my underlying website hosting infrastructure and DNS configuration a bit to avoid breaking it again so easily.

So I converted it all to Terraform, after before most of it used to be set up manually, and will probably look into adding some sort of monitoring for it later.

Both my websites now use a similar setup and I reuse most of the Terraform code, to easily apply changes on both of them across the entire stack.

And while at that, I also spent some time consolidating my email infrastructure. Previously I had it split between Amazon Workmail and Google Workspaces, and I used to pay for both.

Now everything is in Google Workspaces, so I have a single inbox to look into and one less service to pay for, saving a few bucks a month. The use of Terraform made it much easier and faster, for example when creating the MX DNS records, etc, and I also used ChatGPT heavily when writing that new Terraform code, which was a lot of fun.

## Podcast News

Besides this, I also got some news about my [LeanerCloud podcast](https://anchor.fm/leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-27-jan-2023) becoming quite popular. Last week it was 54th in the Technology domain in Israel, which was surprising for such a new and still quite small podcast. This week I didn't record any new episodes, but I'll probably record something on Graviton next week if I feel like it.

Speaking of podcasts, my friend [Jon Myer](https://www.linkedin.com/in/jon-myer?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAAnANSgBJMfP55__raxOVu4a0pziJRRLN7w&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-27-jan-2023) also published a podcast episode we recorded a couple of weeks ago, in which I was a guest speaker. You can check it out [here](https://open.spotify.com/episode/6gXqhbN2ZBmxYoFBma1pBB?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-27-jan-2023). I really loved that conversation with Jon, we covered a bunch of topics that might be of interest, from cost optimization to ChatGPT and the usefulness of AWS certifications, joining communities and building an audience.

## Talking to people

Besides this work, I also had some interesting conversations with a bunch of people about their use of AWS. As usual, I also explored a few possible collaborations or partnerships and still expect to continue this going forward, as there are so many opportunities out there.

## Karpenter support

Out of these conversations I got to talk to someone about Karpenter and helped them a bit with their Karpenter setup. I liked it a lot and this got me thinking to do this kind of work more often going forward.

So if you or some people you know use EKS at scale and are interested in trying out Karpenter, I'd love to help, including with doing some hands on work. Just leave me a DM and let's take it from there.

I'm also gathering data on something to offer in the containers space, probably something built around Karpenter, since I currently don't have an offering dedicated to containers (actually AutoSpotting works great for EKS managed node groups, but Karpenter is in many ways superior to the managed node group model itself). So please let me know if there's anything you'd like to see when it comes to containers and Karpenter but doesn't exist yet.

That's pretty much all for this week, it was another week with lots of progress on multiple fronts and I'm happy how it all turned out.

Thanks for reading this far, have a nice weekend and see you again next week.

-Cristian

#### Keep reading

[![AutoSpotting comparison matrix](/images/blog/leanercloud-content/weekly-update-27-jan-2023/image-3.png)

## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Why I recommended ECS instead of Kubernetes to my latest customer

And how a cost optimization exercise often leads to deeper modernization of cloud applications


## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


View more
