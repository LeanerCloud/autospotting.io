---
title: "Adopting the ONCE Model for All My CLI FinOps Tools and Terraform Building Blocks"
date: 2024-05-27T00:00:00+00:00
description: "Releasing CLI tools and Terraform modules under the 37signals ONCE Model"
author: "Cristian Magherusan-Stanciu"
categories: ["LeanerCloud", "FinOps", "Tools"]
tags: ["leanercloud", "finops", "aws", "optimizer", "terraform", "once-model"]
featured_image: "/images/blog/leanercloud/2024-05-27-adopting-once-model.png"
draft: false
---

I decided to release all the CLI tools and Terraform building blocks I use in my customers engagements under the [37signals ONCE Model](https://once.com?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks), and under their ONCE source-available [license](https://once.com/license?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks).

## What's included?

For now this release only includes Optimizer, my flagship CLI tool for automated optimization of AWS resources which I use all the time in my service [engagements](https://leanercloud.com?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks) to save my customers lots of money.

The main use case for the Optimizer tool is for mass-optimization of any or even all of the supported resources within your AWS account, and it is mainly built for hands-on FinOps consultants or people doing large optimization initiatives like I do for my customers.

It saves you from a lot of time and clicking in the console to look at the CloudWatch metrics, finding the right instance type for you or applying the changes, and can perform multiple actions over a wide range of resources by running a single command.

Main supported actions:

- mass-conversion to GP3 for EC2 EBS volumes and RDS storage, optionally keeping their initial IOPS/throughput characteristics.
- mass-rightsizing with conversion to Graviton where supported for RDS, ElastiCache and OpenSearch resources based on the CloudWatch metrics, applying changes in the next maintenance window by default.
- selection of all the resources based on tags in opt-in and opt-out modes, to limit the scope and blast radius of the mass-conversions.
- plan/apply modes similar to Terraform.

Here's how it looks like in action:

![](/images/blog/leanercloud-content/adopting-model-cli-tools-terraform-building-blocks/image-2.png)

Optimizer plan output I got from it recently at one of my customers

## Other releases soon

Over the next days/weeks I'm working to gradually release over a dozen other AWS cost optimization tools and Terraform building blocks I've been using at my customers to reduce their costs or set up scalable and reliable cloud infrastructure in AWS, here's the list I have so far:

![](/images/blog/leanercloud-content/adopting-model-cli-tools-terraform-building-blocks/image-3.png)

Some of these tools may over time also evolve into stand-alone products which I sell on the AWS Marketplace, but that is not in the scope of this ONCE offer.

## How does it work?

After paying, you will need to send me your Github username, and you will get access to my [LeanerCloud-ONCE](https://github.com/LeanerCloud-ONCE/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks) Github organization, where I'm going to release the full source code of all these tools going forward, including future developments, and you will also get some limited support on the LeanerCloud [Slack](https://join.slack.com/t/leanercloud/shared_invite/zt-xodcoi9j-1IcxNozXx1OW0gh_N08sjg?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks) if you need any help.

## How to get access?

You can purchase access to all these(once I publish them) over a single one time payment of $499 when paid via Stripe or for $699 when purchased through the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-abvqqdkbxpe4q?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks) if you prefer to pay through your AWS bill (it requires more effort on my side and has higher payment fees).

I'm going to gradually increase the price each time I'm adding more tools and occasionally when significantly improving the existing ones, so it will only be available at this price for a limited time.

If you have any questions about this also feel free to reach out to us on Slack, and I'm happy to help.

-Cristian

**Later edit:**

- The bundle was since expanded to also include the Terraform building blocks. and the price was increased to $799, read more about it [here](https://leanercloud.beehiiv.com/p/releasing-additional-terraform-building-blocks-leanercloud-bundle).
- You can now also discuss this on [HackerNews](https://news.ycombinator.com/item?id=40576592&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks) and [Reddit](https://www.reddit.com/r/FinOps/comments/1d82ln9/releasing_my_cli_finops_tools_and_terraform/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=adopting-the-once-model-for-all-my-cli-finops-tools-and-terraform-building-blocks).

.

#### Keep reading

[![Why I recommended ECS instead of Kubernetes to my latest customer](/images/blog/leanercloud-content/adopting-model-cli-tools-terraform-building-blocks/image-4.png)

## Why I recommended ECS instead of Kubernetes to my latest customer

And how a cost optimization exercise often leads to deeper modernization of cloud applications


## Vantage just updated ec2instances.info and released all their code, now what?


## Why I just declined a job offer from a billionaire friend


View more
