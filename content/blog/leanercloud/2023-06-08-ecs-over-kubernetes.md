---
title: "Why I recommended ECS instead of Kubernetes to my latest customer"
seo_title: "Why I Recommended ECS Instead of Kubernetes"
date: 2023-06-08T00:00:00+00:00
description: "Choosing ECS for cost optimization and simplicity at an AI startup"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "ECS", "Kubernetes", "Case Study"]
tags: ["aws", "ecs", "kubernetes", "cost-optimization", "docker", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-06-08-ecs-over-kubernetes.png"
draft: false
---

## And how a cost optimization exercise often leads to deeper modernization of cloud applications


 June 08, 2023

It's been a while since my last post here, and since then I've been pretty busy. I've been working with an innovative AI startup, helping them optimize their AWS cloud setup. At the same time I've been preparing an Udemy course on using ChatGPT for cloud-native software development with a DevOps focus.

In this post I'll talk about my work with this startup, my thought process and what I recommended them, including the potentially controversial recommendation mentioned in the title.

### Initial state of affairs

I was first approached by their CTO a few weeks back. He was concerned about their high burn rate of AWS credits, and wanted to quickly roll out [AutoSpotting](https://autospotting.io?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer) throughout their fleet to extend their credits runway by adopting cheaper Spot instances.

Even though AutoSpotting and Spot instances can work in many situations, I'm not trying to force it where it's not a good fit. We soon realized they're not ready for Spot instances at the moment and scheduled a follow-up deep dive session to see what other options we have to help them reduce their cloud costs.

We usually start such engagements by first going through the latest AWS bill to identify major spend and propose ways to tackle them tailored for each customer.

As I've seen at many other customers before, more than half of their spend is on EC2(quite expected considering the typical computation needs of AI/ML workloads), followed by significant spend for EBS volumes and some for RDS databases. Surprisingly they also had some Amazon MQ, which they use as their main message bus of their microservices architecture with over a dozen microservices running on EC2.

When looking at the cloud resources, we noticed many On-Demand EC2 instances with relatively low CPU utilization, which can be expected considering they don't have customers yet.

We also saw many EBS volumes attached to stopped EC2 instances, about half of their EBS volumes still GP2 that could have been converted to GP3, and a couple of unattached IO2 volumes provisioned with some 4000 IOPS that was costing them a few hundreds dollars monthly.

The RDS was using a relatively large instance with very little usage, and also they had a beefy Amazon MQ, mostly sitting idle but costing them a lot of money.

The application running on EC2 is packaged in Docker containers, and each EC2 instance is checking out code from GitHub and running Docker-compose to build and spin up a number of containers locally.

![Architecture diagram: EC2 instances running docker-compose builds from GitHub](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-2.png)

Initial State

Their current setup with docker-compose is fine when you get started and only have a handful of instances, but once you have a few dozens, each instance ends up wasting significant resources (there's no central per container observability when it comes to resource consumption) and over time you end up with a bunch of "pets", each with a custom configuration different from the others.

Also, having the source code and a bunch of secrets required to build the Docker images on the running instances doesn't follow security best practices.

Developers often turned off instances when they no longer needed them. These instances don't generate EC2 instance costs, but their attached EBS volumes generate costs, and more of these accumulated over time.

### Proposals

We started with some low hanging fruits of changing EBS volumes to GP3 and converting the IO2 volumes to the minimal provisioned IOPS. The customer wasn't interested in using [EBS Optimizer](https://aws.amazon.com/marketplace/pp/prodview-ryzl67mmq3ghk?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer) at the moment so they did this EBS work manually, but this action alone will save them yearly a few times more than all my consultancy fees will cost them.

We also created some action points for later, such as downsizing the MQ instance, converting RDS databases to Graviton and downsizing the databases to match their usage. We may also try the more convoluted process of converting the database to Aurora Serverless v2, but their RDS costs were relatively low so we postponed all this RDS work and started to focus on optimizing the application's EC2 instances, considering that EC2 is their most costly service.

### Ideal scenario for their compute

When it comes to the application, if I had been involved from scratch, I would have recommended SQS and/or SNS for the message bus, which are free of charge at low utilization.

For the compute I would have used Lambda whenever possible (also for negligible costs during the pre-launch phase, although it introduces some complexity), with ECS on EC2 for the relatively few components that need GPU instances.

### Back to the real world

Changing to such an ideal setup would involve significant code changes, making each application to use SQS/SNS instead of MQ and processing Lambda events, so it wouldn't be feasible.

The team didn't have much DevOps expertise in-house, so a Kubernetes setup, even using a managed service like EKS, would have been way too complex for them at this stage, not to mention the additional costs of running the control plane which they wanted to avoid.

My recommendation was to convert the docker-compose setup to ECS, deployed using Terraform.

They had just started looking into Terraform and implementing CI/CD using GitHub Actions, so the proposed setup was also aligned with their longer term vision.

ECS is also relatively simple and not so far from their Docker-compose setup, but much more flexible and scalable. It also enables us to convert their somewhat stateful pets to identically looking stateless cattle that could be converted to Spot instances later.

Using Terraform and GitHub Actions also forces us to build the Docker images and push them to ECR from GitHub, so we no longer need the full source code and secrets to be available on the EC2 instances at runtime, which improves the security posture of the application.

ECS will also offer ECS container logs and metrics out of the box, giving us better visibility into the application and enabling us to right-size each service based on its actual resource consumption, in the end allowing us to reduce the number of instances in the ECS cluster once everything is optimized.

We chose to use ECS on EC2 because of their need for GPU instances for some of the workloads. Using EC2 also has lower runtime costs compared to Fargate, more simplicity than a mix of EC2 and Fargate and more flexibility than Fargate when it comes to sizing the tasks.

The idea was to initially provision EC2 instances with the largest available memory to CPU ratio to maximize their utilization at their current low CPU usage.

In the ECS world we can also adopt Spot instances with AutoSpotting for a good part of their workloads, and once everything is figured out, purchasing Savings Plans or RIs for remaining capacity and the database hosts.

![Proposed ECS architecture with Spot instances and Savings Plans](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-3.png)

Proposed state

### Current state

I soon provided the team with some examples on how to set up ECS clusters and services from Terraform, and instructions on how to configure Terraform remote state and to integrate everything into GitHub Actions for CI/CD, accelerating their IaC and CI/CD implementation.

We then used Terraform to create an ECS cluster using instance types with a high memory to CPU ratio, and started to convert the docker-compose configurations to ECS services, also deployed from Terraform. At first we did it all manually, until we got a service running correctly, to see how the desired configuration looks like.

The Terraform code was extended based on the [docker-build](https://registry.terraform.io/modules/terraform-aws-modules/lambda/aws/latest/submodules/docker-build?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer) pattern for Lambda, so that it builds and pushes Docker images to ECR from the same Terraform code that then configures the ECS service. This avoids multiple build steps and passing configuration around, allowing us to have a simpler CI/CD pipeline and no configuration drift, everything being set up and deployed from a single Terraform "apply" command executed from Github Actions.

Once we saw how the ECS Terraform configuration looks, with help from ChatGPT I then also created a script that automatically converts docker-compose configuration files to Terraform code that creates equivalent ECS services. I also delivered this script to the team to speed up conversion of their other microservice configurations.

The team soon started to convert more docker-compose files to services deployed from Terraform, and to integrate them in their GitHub Actions.

### Challenges

After configuring a handful of the microservices, we soon noticed multiple instances spinning up in ECS, and each instance only running a single container, even though there was plenty of capacity available for multiple containers on each container host.

After investigating, it turned out that the ECS configuration had a port mapping with fixed TCP ports, and since each container was listening on the same port for its Prometheus configuration (I wasn't aware initially, but the team had started to use Prometheus for some application metrics), and because ECS was configured with the "bridge" networking mode to mimic what they had in docker-compose, the containers clashed with each other and ECS could only schedule a single task on each host.

The solution for this issue was to configure the recommended "[awsvpc](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-networking-awsvpc.html?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer)" networking mode, which allows us to avoid such conflicts, among a multitude of other benefits.

### Next steps

The team will continue converting their microservices to ECS using the script we provided as a starting point. For each of the microservices we will later evaluate the resource consumption, and based on the task metrics we will right-size each of the tasks.

Once each microservice is converted and verified to work correctly, we will terminate all its previous EC2 instances, including the previously stopped ones in order to free their EBS volumes.

In parallel to this microservice refactoring, we will also convert the new ECS setup to Spot instances using AutoSpotting, so everything we move to ECS will be optimized for lowest costs out of the box, and this change alone should bring some 50-60% savings on their EC2 spend, on top of the right sizing and EBS savings we're doing.

We will also keep an OnDemand capacity provider for things not suitable for Spot instances.

The plan is to also purchase Savings Plans or RIs for any remaining On-Demand capacity, considering also that GPU capacity may not be available in large enough numbers as Spot or will lead to a large number of interruptions.

We will also purchase RIs for their database once it's right-sized and converted to Graviton or Serverless v2. Unfortunately, it seems MQ isn't supported by RIs or Savings Plans at the moment so there's not much we can do about that, except for right-sizing it.

### Final words

This is still an ongoing project and I'm looking forward to see the end results of all this work and hope the customer will be satisfied with my services and the savings generated by AutoSpotting.

When it comes to the required effort, so far I was involved hands-on for a couple of half-days spent with the CTO to brainstorm the solution, spin up the ECS cluster and figure out the configuration of the first microservice. Then we delegated the rest of the work to their engineers, who so far spent a few more days converting their microservices, and I remain available as stand-by in case they need more help.

I would be disappointed if the work we did here didn't have a 5x return on my fee plus their time/effort investment in the first year, and that doesn't include the many improvements to the deployment pipelines and security posture of the environment we made along the way.

That's it for now, stay tuned for more updates.

-Cristian  
  
P.S. If you know of anyone who may need help with such deeply technical infrastructure modernization or cost optimization work(I love helping startups extend their runway, especially those in the AI space), please [send them my way](https://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer), and I'm happy to help.

P.P.S If you liked this piece and want to get more of this you can sign up or see previous [posts](https://leanercloud.beehiiv.com/), check out my **[YouTube channel](https://www.youtube.com/@Leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer)****,** [**Podcast**](https://anchor.fm/leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer)**,** follow me on [Twitter](https://twitter.com/magheru_san?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer) or connect with me on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-i-recommended-ecs-instead-of-kubernetes-to-my-latest-customer).

#### Keep reading

[![Why Kubernetes wasn't a good fit for us](/images/blog/leanercloud-content/recommended-ecs-instead-kubernetes-latest-customer/image-4.png)

## Why Kubernetes wasn't a good fit for us

My thoughts on when to Kube or not to Kube...


## Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks


## AutoSpotting comparison matrix

Easily see the difference between ASGs and the available AutoSpotting options.


View more
