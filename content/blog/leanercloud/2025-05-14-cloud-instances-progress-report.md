---
title: "Progress Report for the First Week After Forking ec2instances.info"
seo_title: "cloud-instances.info: First Week Progress Report"
date: 2025-05-14T00:00:00+00:00
description: "First week of development on cloud-instances.info fork: domains, hosting, and community response"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["cloud-instances", "ec2instances", "aws", "cloudflare", "terraform", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-14-cloud-instances-progress-report.png"
draft: false
---

It's been a little more than a week since I decided to fork [ec2instances.info](http://ec2instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) as [cloud-instances.info](https://cloud-instances.info/index.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info), which I [announced](https://leanercloud.beehiiv.com/p/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) publicly last Thursday.

While in my previous post I mostly focused on the context around this fork and how I got to it, on this post I want to talk a bit about what happened after after I decided to fork the project over the last few days and where we're at right now.

In between the decision to fork and the announcement I worked around the clock to prepare everything, and it took me less than 48 hours to have the fork public and announced.

## The domain

First thing was deciding on a domain, since that's going to be used for the website, but also for the name of the repo, and all over the configuration.

These were my criteria for the domain:

- something generic and descriptive of a website about cloud compute instances.
- not tied to any cloud provider or FinOps tooling vendor, including my own [leanercloud.com](http://leanercloud.com?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) domain.
- similar enough to the initial domain [ec2instances.info](http://ec2instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info), ideally keeping the .info domain.
- available to buy for a cheap cost, since at the moment I can't afford to pay thousands of dollars on a domain.

In the end I decided for [cloud-instances.info](http://cloud-instances.info/index.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info), which fits all those criteria pretty well, and bought it on Namecheap for only $3.16 for the first year:

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-2.png)

## The git repository

With the domain secured, the next thing was to fork the repository.

For now it lives under my own LeanerCloud github organization but we may move it to its own organization later once we have multiple co-maintainers.

Once I forked the repo, I attempted to build the code, which failed with some weird errors.

It turns our Vantage might have a private fork that besides their Marketing CTAs and Azure support code also includes some build fixes that aren't available in their open source version.

I did a [few](https://github.com/LeanerCloud/cloud-instances.info/commit/e607f2cdb578d9aac981c307a7e7752f161c0a73?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) [small](https://github.com/LeanerCloud/cloud-instances.info/commit/918bc3c3bfd5c00dab66ae0098dbe766213cee70?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) [fixes](https://github.com/LeanerCloud/cloud-instances.info/commit/db0ea64a68a63b71cf30b9160bb09e37a6c9f616?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) that finally got it to build, then I wanted to host it as a website, since it's not very useful to anyone if I just run it from my own machine.

## The website

While the domain and the git repo were quick and easy, I struggled a lot with the website.

Hosting it in S3 and CloudFront would have been easy, but I would rather not have to pay the hefty data transfer fees from my own pocket once this thing gets enough traction.

So I chose to run the new website on CloudFlare, since they're the only main CDN I know of that offers a generous free tier for static websites, and also supports IaC, ideally from Terraform.

## CloudFlare issues I ran into when running the website

### CloudFlare Pages is not an option for our large files

At first I was looking into CloudFlare Pages, a fully managed static website service that just needs a git repo and simple configuration to host a website.

It seems exactly what I needed, but unfortunately CloudFlare Pages has a maximum file size limit of only 25MB, which wouldn't be enough for some of our large instance type information JSON files, some of which approaching 100MB.

The alternative was using R2, the S3 compatible object storage alternative offered by CloudFlare, which can be used as CDN origin.

It supports Boto with very few changes to our deployment code, so in the end it was easy to get our files uploaded with just a few tweaks to our deployment code after attempting a few other solutions that weren't working so well.

This also allowed us to push objects in nicer URLs, like `/about/index.html` as `/about` and so on.

### CloudFlare R2 issues

One problem with R2 I ran into very quickly was you can't have an `/index.html` file configured for the root of the website like we have in S3.

And if you try to use `/` as the object name for the `/index.html` then the website would have to be loaded using `https://cloud-instances.info//` (notice the double slashes), and there's no way to have an R2 object with empty key which would have been needed to remove the second slash.

The workaround for that was using a simple [CloudFlare worker](https://github.com/LeanerCloud/cloud-instances.info/blob/master/infra/terraform/cloudflare_environment/worker/src/index.ts?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) that does the redirect from `/` to `/index.html` while keeping the query parameters.

This is not so nice since all URLs now need to have `/index.html`, which is fugly, but I'd rather have that than paying the egress costs from my own pocket once this thing becomes popular.

Unfortunately this `/index.html` situation also broke the website when switching to other regions, since the code tries to load regional data using links like `/regional-JSON-file.json`, which are stored in S3, but then with the `/index.html` they would be downloaded as `/index.htmlregional-JSON-file.json`, which as you can imagine doesn't exist in our R2 bucket.

This was immediately noticed by someone after the announcement and I fixed it in the JavaScript code that creates those links, which are now created correctly, and that's the only such issue we know of so far.

At first I tried to deploy my CloudFlare worker using the CloudFlare Wrangler tool, but then I wanted to port the configuration to Terraform, which I'm more familiar with and also more likely to be familiar with other potential contributors.

Another reason for using Terraform was that I want to have a way to create ephemeral environments for pull requests, which would enable us to develop it in the open.

### Terraform "fun"

Well, it turns out the CloudFlare Terraform provider is in a state of flux.

Even simple things like creating the R2 bucket aren't supported in the current version 5.4.0, and this was broken for the last couple of versions. The release for version 5.5.0 currently still fails to compile and has been failing for the last week or so since I first looked into this.

I tried previous versions that still supported bucket creation, but then they were broken when creating the CloudFlare Worker or CloudFlare Zone.

In the end I announced it while the Terraform code was still broken and hoped to fix it later.

## The announcement post

Wrote the announcement post late that night and posted it around 2AM, knowing that the next day I won't have time for working on this.

It was going to be a public holiday and I had to spend most of the day with my son, and I didn't want to delay this into the weekend. I wrote it quickly as I could, with a bunch of typos I fixed the next day, and briefly posted about it on LinkedIn and Reddit then went straight to bed.

## Reactions to the announcement

The announcement was quite well received by the community, with many people expressing their support and saying they're going to update their bookmarks.

A few people noticed the broken links issue which I fixed or documented immediately in between spending time with my son that day.

Someone more familiar with this CloudFlare situation suggested mixing the Terraform providers, using an old version for creating the bucket, and to create the rest of the resources with the current version, which luckily supports everything else. Thank you Pujan!

This finally worked and allowed me to fix my IaC setup, yay!

Someone working at BMW as FinOps Engineer (hi Syed if you read this!) offered to contribute code changes the very next day, and already has a couple of contributions in the linters and CI/CD pipeline.

For comparison, when I first released AutoSpotting in 2016 it took me 2-3 months until I got a first user brave enough to try it out, and about 5 months until I got the first pull few requests.

To be honest all this happened so fast that I'm still struggling to process it all.

## Communication with Vantage

I've also been talking to my Vantage contacts on and off throughout the entire time. While they're not supportive of the fork, they appeared friendly and acknowledged that it's my right to fork the project.

It turned out the entire ghosting situation I mentioned in the announcement post was caused by one of my Vantage contacts having a newborn before the expected due date and failing to set an autoresponder.

Unfortunately it was too late to revert everything, so I will continue working on this fork as planned. For me it's a critical tool in my day to day work and I want to have a say in how it's being developed.

I offered Vantage to join forces and consider this as an upstream project, while they can keep their private fork with their own Marketing CTAs, but I haven't got any answers from them since.

## Other FinOps vendors offering to sponsor development

Over the last few days multiple FinOps tooling vendors started to offer to sponsor me to work on this project and suggested to meet at FinOpsX in a few weeks.

I'd love to accept all those sponsorships, since after all when I started to build AutoSpotting one of my hopes about it was for it to potentially become a sort of cash cow project that would support me in working full time on all sorts of other open source projects I may feel like building following my curiosity.

I failed miserably at that, which got me to join AWS, then on my own to give it another try, failing again to monetize it as OSS and pivoted to services about 2 years ago.

From the looks of it seems like the current situation is unfolding towards that outcome, almost a decade later, which I'm very excited about.

I just want to make sure I'm doing it in a fair and sustainable way that doesn't compromise my vision for this project to remain a vendor-neutral tool and efficient cloud instance type database developed fully in the open.

I haven't established all the details about how this would work, but at the moment I'm talking to the multiple vendors who offered sponsorship and trying to come up with something that works for all of us.

If you're interested to sponsor my work on this in a way aligned to this vision(or my attendance to FinOpsX in a few weeks), please reach out to me on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) and let's take it from there.

## What I've been working on so far

For the first few days I've been working on the fork all by myself, doing a few dozens commits for fixing the build, preparing the environment for launch by deleting Vantage marketing CTAs, and the hosting infrastructure.

I enabled all sorts of security features on the repo such as linters, static code analysis checks, checks for outdated dependencies, etc.

After the announcement, with help from the community I came up with a workaround for the Terraform provider issues, cleaned up and published all the Terraform code I have so far, as well as the deployment code changes that make it all work on CloudFlare.

Then kept evolving the CI/CD configuration towards enabling us to have temporary environments for each pull request. This way we can develop things in parallel completely in the open.

This is still work in progress, but the temporary environments are created for each pull request:

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-3.png)

For now the cleanup action still fails because we need to empty the bucket before we attempt to delete it, but that should be an easy fix:

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-4.png)

Now that I think of it I should probably randomize the bucket names as well for security reasons.

## Coming up next

I first want to finish the ephemeral environments work, which is almost done.

Once this is done I want to address the security scanners, making sure everything is nice and tidy since security is always job zero.

Then want to start working on the Azure code that is the main gap we have from Vantage's private fork at the moment.

This should keep me busy for the next few weeks, considering that I also have other work to do and can only allocate to this maybe 10-20% of my time for the next few weeks until hopefully sponsorships from will allow me to spend more time on this over working for my cost optimization clients.

I also created a number of issues on GIthub and I hope to see the community stepping up and contributing some of those, until I can get to them myself, and feel free to also have a look at the existing 97 [issues](https://github.com/vantage-sh/ec2instances.info/issues?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=progress-report-for-the-first-week-after-forking-ec2instances-info) from the Vantage repo.

![](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-5.png)

That's all for now, I'm very excited to work on this project, so stay tuned for further updates soon.

-Cristian

#### Keep reading

[![My thoughts on the current state of EC2 Spot pricing](/images/blog/leanercloud-content/progress-report-for-the-first-week-after-forking-ec2instances-info/image-6.png)

## My thoughts on the current state of EC2 Spot pricing

And what you can do about it


## Current OpenTofu contributors vs. pledged FTEs

OpenTofu Github stats half a year after the fork.


## Why I recommended ECS instead of Kubernetes to my latest customer

And how a cost optimization exercise often leads to deeper modernization of cloud applications


View more
