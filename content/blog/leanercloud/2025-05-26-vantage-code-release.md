---
title: "Vantage Just Updated ec2instances.info and Released All Their Code, Now What?"
seo_title: "Vantage Released the ec2instances.info Code - Now What?"
date: 2025-05-26T00:00:00+00:00
description: "Analyzing Vantage's refreshed ec2instances.info and planning the future of cloud-instances.info fork"
author: "Cristian Magherusan-Stanciu"
categories: ["Open Source", "Tools"]
tags: ["ec2instances", "cloud-instances", "vantage", "open-source", "leanercloud"]
featured_image: "/images/blog/leanercloud/2025-05-26-vantage-code-release.png"
draft: false
---

In case you haven't noticed, Vantage recently updated [ec2instances.info](https://ec2instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what) with a new refreshed look and feel.

Over the weekend I had a deep-dive into their changes and thought about what to do with the [cloud-instances.info](http://cloud-instances.info?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what) fork I [launched](https://leanercloud.beehiiv.com/p/forking-ec2instances-info-as-a-vendor-neutral-alternative-at-cloud-instances-info) just a few weeks ago.

## Why I forked it in the first place?

I covered this in more depth in my initial post, but basically when I decided to fork it, their main public code base hadn't been touched in the last 6 months, and barely touched in the last year, except for a couple of my pull requests.

I suspected they had a private branch with their marketing changes, and was suspecting also some fixes on the underlying scraper code, which has been broken in the public code base for months but every time I noticed the breakages in the public code their version was still showing refreshed data.

Later it turned out their Azure code was also only developed in that private branch and never open sourced, and when I looked into how to fix some Azure issues that code was nowhere to be found in the public repos.

I then learned they were rebuilding it from scratch in private, but it wasn't clear what they're doing:

- how would the rewrite differ from the initial version?
- do they keep the same underlying data source(which I rely on for many of my tools)?
- will they keep working on it afterwards or will they again fizzle out after a while?

On top of those, as any rewrite from scratch, I was expecting a bunch of issues to show up after the fact that will need time to be ironed out.

When I asked them about these things they weren't able to answer, so I decided the safer option for me is to fork it.

In case they'd mess it up entirely at least I'd have a mirror of the initial version, without their marketing message at the top of the page that was annoying to see every day, and with the same data source which I can keep using for my tools going forward.

And if they do decide to release a new version and release all their private code changes, I'd carefully rebase my code on top of that, at my own pace, so I can mitigate any breakages.

They just released the new version the other day, so over the weekend I had some time to look into it and to do some thinking on what to do next, and I'll try to talk about all this below.

## What have they released?

They have an entirely new frontend code, with a more modern look and feel.

Lere's how it looks like now:

![The ec2instances.info homepage after Vantage's update](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-2.png)

When it comes to the way it looks, the main visually noticeable changes are in the header. They removed that obnoxious marketing banner from the top of the page which was very annoying for people using this tool for work on a daily basis. Instead of that they just left a discreet "presented by Vantage” note which is completely fine to me.

![The discreet 'presented by Vantage' note on ec2instances.info](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-3.png)

As you can see they also moved the Azure entry point at the top of the page, which makes it easier to discover compared to the previous version, where we used to have a toggle to Azure at the bottom of the page.

I really like this as well!

On the flip side, there are a few small changes in the way the filters work, which were already raised by users and I expect to be fixed soon. Basically the CPU and memory filters are broken.

Another regression I noticed was the sorting by price shows unavailable instance types first:

![Sorting by price shows unavailable instance types first](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-4.png)

This isn't such a big issue in Virginia, where most instances are available, but in other regions you have to scroll through a lot of unavailable instance types before you get to some of the available ones, as you can see below in London:

![Many unavailable instance types listed before available ones in the London region](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-5.png)

And the last thing I noticed was the removal of the region search field from the top of the region drop-down list, which makes it harder to find the region you want to look at:

![Region drop-down list missing the region search field](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-6.png)

Compared to what we have in the old version:

![The old ec2instances.info region drop-down with search field](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-7.png)

Looking under the hood, the data which used to be pre-rendered as HTML at build time is now fetched and only rendered from Javascript for the current view, which makes it load much faster compared to the previous implementation.

Looking at the source of the page I noticed they're now using using React, which is a much more modern way of building webapps, compared to the pre-rendered HTML we had for ages.

I also spent some time to look at the repo and I can now also see their Azure code is now available as open source, yay! And they also released the Github Actions they use to build and deploy the code!

Looks like they also now host it on Cloudflare in a similar way to how we do it, with R2 and Workers (previously it used to be hosted on S3+CloudFront if I remember correctly), although apparently in a different way from how we do it in the new fork.

They figured out how to avoid the `index.html` issue we ran into, and for deployments they use the Wrangler github action “cloudflare/wrangler-action", whereas we do the deployments with Terraform.

To be honest I prefer their wrangler approach for simplicity, although Terraform gives us more flexibility when it comes to the ability to create dynamic environments for pull requests, which is one of the things we attempted to do in the fork, but haven't finished doing yet.

Besides the Azure code, they also released all their previously private code changes, including their previously unpublished Azure support code, scraper fixes and infrastructure code.

I also had a brief look at the EC2 scraper, and it seems to have barely changed, except for a few changes that add support for recently released GPU instance types, which are hardcoded in the scraper. In my fork at least I took those out of the scraper into a dedicated file that should make it easier to maintain. This makes their data source format compatible to what we have as well in the fork.

## What now?

I applaud most of these changes, and I'm sure the minor sorting UI/UX issues will be sorted out soon (pun intended! 😀)

But while I very much welcome this release, it also puts me in a kind of awkward situation.

On one hand it really shows how my fork influenced the way they did many of these things (publishing the Azure code and scraper fixes, removing the banner after I complained about it, using Cloudflare to avoid hosting costs, publishing the Github Actions, etc.), so I'm happy that it put some pressure to get these things in the right direction.

As per my initial concerns, I'm also glad that I still have my fork until they fix those UI/UX issues, although I expect that to not take so long.

But on the flip side, I'm also in a somewhat tricky situation regarding the things that happened in the meantime.

I had people and companies start sponsoring me on Github for working on this fork going forward, and people sponsored me to attend FinOps X next week after I announced the fork. I'll have to talk to each of those people to see how this Vantage release changes their sponsorship intentions, and if I now need to refund them, which would really suck.

There's also the work myself and another contributor put into that fork, a lot of which is now somehow duplicating things that Vantage just published, and feels a bit sad to see all that effort duplicated and thrown away, although it was fun and I learned new things while doing it.

In hindsight I think I may have been able to put that kind of pressure in a different way, through dialogue, instead of doing a hard fork.

But what's done is done, there's no way to turn back time, only to influence the present and build a better future.

## Next steps

In a nutshell I decided to keep the fork and maintain it for the foreseeable future.

On one hand, the new Vantage website still has those UI/UX issues, and it's nice to have the old version which just works as before.

Going forward I want to keep the site as a vendor-neutral tool, much like we have the Chromium and VSCodium projects for Google Chrome and VSCode. I’m still friendly towards the Vantage team and hope we can find ways to collaborate going forward in such a setup.

After all, I have people who sponsor me to maintain it as a vendor-neutral alternative, which I intend to keep doing until further notice. And if more people want to sponsor development I'm happy to accept [sponsorships](https://github.com/sponsors/cristim?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what) that go towards me allocating more time for open source development work, and not only just for this project.

In the near future I will keep the fork based on the old code base until all of those UI/UX issues are sorted out by Vantage. So if those bother you, feel free to use [cloud-instances.info](https://cloud-instances.info/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what) in the meantime.

Once those UX issues are sorted out by Vantage I want to gradually incorporate these recent changes into the vendor-neutral fork, and submit our changes that they may find useful as pull requests (we have some nice improvements that speed up the build time for software changes by caching the scraped data).

Eventually I want to contribute meaningful changes to the Vantage repo, trying to join forces towards building this tool together, in the interest of the users and the open source community, while keeping this unbranded fork as an alternative for whoever wants to use it.

That's all for now, stay tuned for further updates.

-Cristian

#### Keep reading

[![Current OpenTofu contributors vs. pledged FTEs](/images/blog/leanercloud-content/vantage-just-updated-ec2instances-info-and-released-all-their-code-now-what/image-8.png)

## Current OpenTofu contributors vs. pledged FTEs

OpenTofu Github stats half a year after the fork.


## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


## Why I just declined a job offer from a billionaire friend


View more
