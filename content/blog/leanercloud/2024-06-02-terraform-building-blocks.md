---
title: "Releasing Additional Terraform Building Blocks to the LeanerCloud ONCE Bundle"
date: 2024-06-02T00:00:00+00:00
description: "Terraform serverless microservice modules added to LeanerCloud ONCE bundle"
author: "Cristian Magherusan-Stanciu"
categories: ["LeanerCloud", "Terraform", "Tools"]
tags: ["leanercloud", "terraform", "serverless", "aws", "lambda", "ecs", "once-model"]
featured_image: "/images/blog/leanercloud/2024-06-02-terraform-building-blocks.png"
draft: false
---

Announcing a new release to the [LeanerCloud](https://leanercloud.beehiiv.com/p/adopting-model-cli-tools-terraform-building-blocks) [ONCE](https://leanercloud.beehiiv.com/p/adopting-model-cli-tools-terraform-building-blocks) bundle, which consists of the Terraform serverless microservice building blocks.

The bundle also contains the Optimizer CLI FinOps tool for GP2 to GP3 conversion for EC2 and EDS, and rightsizing with conversion to Graviton for RDS, Elasticache and OpenSearch, released previously.

These Terraform building blocks are a set of example Terraform modules for building serverless applications using Lambda and ECS Fargate. They will have a slightly different license from the CLI tools, which will allow you to use them to build proprietary products and use within your team, which is not permitted for the CLI tools.  
  
We have a layered architecture, in which each layer is creating its own resources using standard Terraform modules, and publishing some values to SSM parameters so they can be consumed by the higher level components without the need for configuration.  
  
We have a naming convention for the SSM parameter path that makes it easy to use this code across multiple application environments in the same account.  
  
The Lambda module includes an example for deploying Lambda with Function URLs in this deployment pattern, behind a CloudFront distribution, with ACM certificate provisioned through DNS verification and using a Route53 DNS record.  
  
For ECS we have a way to deploy a shared ECS cluster with shared load balancer, stand-alone from the application Terraform stacks. The load balancer similarly has TLS configured with an ACM certificate, and a shared Router33 wildcard record.  
  
Then, on top of the cluster you can have any number of ECS services that can be deployed independently, but sharing the cluster and adding themselves to the load balancer target group and listener using name-based virtual hosting.  
  
There's also a module for deploying an RDS Aurora Serverless v2 database using this model, and automatically wiring it into the application, decrypting the secrets and injecting them as environment variables to the ECS task configuration.  
  
We also have CI/CD configuration with Github Actions out of the box for all the components, as well as configuration for enabling IAM authentication for the entire Github organization, which is used for the Github Actions CI/CD configuration.  
  
Considering the complexity and additional value provided by these Terraform building blocks, the price of the LeanerCloud ONCE bundle increases by $300 for both Stripe and AWS marketplace to $799 and $999 respectively.

You can (for now) purchase perpetual access [here](https://buy.stripe.com/00gdSLeVd5pg8AU9AB?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=releasing-additional-terraform-building-blocks-to-the-leanercloud-once-bundle). The purchase link will become invalid when we increase the price after releasing new components of the bundle and we'll always provide a new one when announcing that new release.

![](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-2.png)

#### Keep reading

[![My thoughts on the current state of EC2 Spot pricing](/images/blog/leanercloud-content/releasing-additional-terraform-building-blocks-leanercloud-bundle/image-3.png)

## My thoughts on the current state of EC2 Spot pricing

And what you can do about it


## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


## Forking ec2instances.info as a vendor-neutral alternative at cloud-instances.info


View more
