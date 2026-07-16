---
title: "Weekly Update - 6 Jan 2023"
date: 2023-01-06T00:00:00+00:00
description: "New year kickoff with AWS Marketplace reporting tool development and EBS Optimizer updates"
author: "Cristian Magherusan-Stanciu"
categories: ["Updates", "EBS-Optimizer"]
tags: ["weekly-update", "aws-marketplace", "chatgpt", "ebs-optimizer", "leanercloud"]
featured_image: "/images/blog/leanercloud/weekly-updates.png"
draft: false
---

![](/images/blog/leanercloud-content/weekly-update-6-jan-2023/image-2.png)

Hi, Happy New Year, and welcome to the first weekly update from 2023 and the first one on Beehiiv!

For more context, this is a continuation of my older AutoSpotting, Cloudutil newsletters, and going forward will mirror my current [LinkedIn](https://www.linkedin.com/newsletters/my-journey-6982031920998006784/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=weekly-update-6-jan-2023) newsletter which I've been using for weekly updates for the last few months after I left Amazon AWS. You can check out my previous posts on LinkedIn.

## Slow start of the year

This was another slow week. We had guests for the week around the New Year and again our child was at home for half of the week. Over the holidays I took a lot of time to decompress and spend with my family. I also resumed learning to play piano and guitar and had a lot of fun at it.

I only started working on Thursday, after our guests left and our son got back to kindergarten. I spent most of Thursday reaching out to people on Slack or LinkedIn and improving my Notion CRM. I also started booking calls with friends, existing customers and a few prospects.

## Writing a reporting tool for the AWS Marketplace

Thursday evening, after so much time spent talking to people, I got in the mood for writing some code. But it was already late so I didn't feel like immersing in my big projects but work on something small and easy.

A while back I got an idea to build a little script for processing the AWS Marketplace daily reports. I like to keep track of my sales numbers on a daily basis, which is a royal PITA.

If you can imagine, the AWS Marketplace has no dashboard for sellers, only dropping some CSV files in S3. This requires logging in to the AWS console, going to an S3 bucket, downloading files. Then the files are simple CSV files with numbers, without any aggregation or trends. Also there's no easy way to identify the customers, unless looking at many files.

I find this ridiculous, and I raised this many times but nobody seems to care about such stuff. And with the recent layoffs at AWS, I wouldn't hold my breath for it to happen anytime soon.

Doing this daily takes a lot of time so I decided to scratch my own itch and automate this tedious piece of work, parse these files and generate a nicer report. It shouldn't be hard considering how crappy this is. The goal is getting a meaningful daily report with only a few keystrokes, or even automated.

So with a lot of help from ChatGPT, I started writing a little script to download and then parse a CSV file into a Go structure. After a few iterations and manual tweaks to the generated code I got it to compile and print some data parsed from one of my CSV files. There are more files, which I need to process the same way, and then combine those data structures into a pretty report, which I'll probably send by email.

I won't paste the code or output here, but it was surprisingly good and pretty close to what I needed it to do. I wouldn't trust ChatGPT for development in a dynamic language, but Go just won't compile if there are clear errors, and then it's easy to take over and refactor the stuff. But it beautifully handles boilerplate stuff like authenticating to AWS, downloading from S3, parsing a CSV file, etc. It's like Github Copilot on steroids.

It's far from ready yet, but it was a fun project so far, I now have the basics and I'll keep working on this between my other bigger tasks.

I'm looking forward to see how this will evolve. Once I'm happy with it for my own use I may even sell this as a solution on the Marketplace. Considering how crappy this workflow is, I bet someone out there would be willing to pay for this a few bucks a month.

## EBS Optimizer work

Today I then started working on EBS Optimizer, preparing for a new release on the AWS Marketplace. I spent some time testing, tweaking the build system and some underlying infrastructure. Then I started simplifying the product description, moving a lot of details to the FAQ.

I'm still not done and I'll continue with this over the weekend or even early next week. But the plan is to have a new release on the AWS Marketplace ASAP, so I can start working again on the GUI next week, as I keep getting feedback that there should be a GUI that estimates savings before running my tools.

That's all for now, thank you for your attention, I hope this content was useful and stay tuned for the next week's update.

And again, Happy New Year!

-Cristian

#### Keep reading

[![Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle ](/images/blog/leanercloud-content/weekly-update-6-jan-2023/image-3.png)

## Releasing additional Terraform building blocks to the LeanerCloud ONCE bundle


## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


View more
