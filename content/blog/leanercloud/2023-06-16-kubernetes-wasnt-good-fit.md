---
title: "Why Kubernetes Wasn't a Good Fit for Us"
date: 2023-06-16T00:00:00+00:00
description: "Understanding when Kubernetes complexity outweighs its benefits for small teams"
author: "Cristian Magherusan-Stanciu"
categories: ["AWS", "Kubernetes", "ECS"]
tags: ["kubernetes", "ecs", "aws", "infrastructure", "leanercloud"]
featured_image: "/images/blog/leanercloud/2023-06-16-kubernetes-wasnt-good-fit.png"
draft: false
---

## My thoughts on when to Kube or not to Kube...


 June 16, 2023

Hello again,

In my previous [post](https://leanercloud.beehiiv.com/p/recommended-ecs-instead-kubernetes-latest-customer) I explained the reasons why we ended up choosing ECS instead of Kubernetes for my current [LeanerCloud](https://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us) customer.

That post was mostly focused on the customer's needs and how ECS was a better fit for them.

That blog became pretty viral with over 20k views so far and I received a lot of feedback saying that it was contrarian, and that I should have talked more about where Kubernetes makes sense and why it didn't make sense for us at this stage.

I agree that I barely mentioned Kubernetes, so in this post I'd like to focus more on why Kubernetes wasn't a good fit for us, or at least not yet.

*TL;DR: 'because our workload is so small that he overhead in cost and maintenance is big. Also, we don't really need all the features, like privilege separation, that Kubernetes provides'. -* ***[rlnrlnrln](https://www.reddit.com/user/rlnrlnrln/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us)***

But first let's discuss a few basics about Kubernetes and how it differs from ECS.

## Kubernetes architecture

Architecturally, Kubernetes consists of a control plane running a number of internal services, and a number of worker nodes that run your applications.

Both the control plane and the nodes run a number of components, as you can see below:

(I'd recommend the [DevOpsCube article on Kubernetes architecture](https://devopscube.com/kubernetes-architecture-explained/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us) for a much deeper dive into this, I'll just cover the basics)

![](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-2.png)

Image from [https://devopscube.com](https://devopscube.com/kubernetes-architecture-explained/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us)

Cloud installations of Kubernetes are usually managed by a cloud provider such as Amazon's EKS service, which takes care of the internal complexity of the control plane itself, such as encrypting the storage of the etcd database, securing the API server, applying security patches, etc.

The deep integration of the applications to the cloud provider is done by a variety of so called controllers, responsible for creating and managing load balancers, EBS volumes, DNS records, etc.

Controllers are the building blocks that implement the functionality of all the usual Kubernetes objects developers love so much.

![](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-3.png)

Image from [https://devopscube.com/](https://devopscube.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us)

## The pros and cons of the Kubernetes architecture

This architecture is very flexible and powerful, you can do a lot of very fancy things with a plethora of addons available from the vibrant Kubernetes ecosystem.

It can also be a much more pleasant experience for developers to work with high level Kubernetes objects, like adding a one-liner annotation to a service to get a DNS record and another one for a load balancer as shown below:

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: "nlb"
    external-dns.alpha.kubernetes.io/hostname: "my-service.my-domain.com"
spec:
  type: LoadBalancer
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

Compare those elegant one-liner annotations to the ECS way of writing relatively more verbose Terraform code to create the same DNS record and load balancer directly:

```
# AWS Load Balancer
resource "aws_lb" "test" {
  name               = "test-lb"
  internal           = false
  load_balancer_type = "network"
  subnets            = ["subnet-abcde012", "subnet-bcde012a", "subnet-fghi345a"]

  enable_deletion_protection = true

  tags = {
    Name = "test-lb"
  }
}

# AWS Route53 DNS record
resource "aws_route53_record" "www" {
  zone_id = "Z2ABCDEF01234"
  name    = "my-service.my-domain.com"
  type    = "A"
  alias {
    name                   = aws_lb.test.dns_name
    zone_id                = aws_lb.test.zone_id
    evaluate_target_health = false
  }
}
```

But that's not the full picture.

The problem is that on top of the costs of the control plane itself(which some cloud providers still charge for) and some small compute capacity needed to run the controllers, those controllers need to be installed and maintained by someone.

Yes, about a dozen of controllers are available out of the box from the managed service in case of Amazon EKS, but still some of them need to be installed by a cluster maintainer.

Here's how the installation process for the controllers that power the above annotations may look like on a fresh EKS cluster:

```
# Download Helm
curl -sSL  https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Add the EKS repository to helm
helm repo add eks https://aws.github.io/eks-charts

# Create a namespace
kubectl create namespace aws-load-balancer-controller

# Install the AWS Load Balancer Controller Helm chart
helm install aws-load-balancer-controller \
  eks/aws-load-balancer-controller \
  --namespace aws-load-balancer-controller \
  --set clusterName=<your-cluster-name> \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \ 
--set vpcId=<your-vpc-id> --set region=<your-region>

# Add the Bitnami repository to Helm
helm repo add bitnami https://charts.bitnami.com/bitnami

# Create a namespace
kubectl create namespace external-dns

# Install the ExternalDNS Helm chart
helm install external-dns bitnami/external-dns \
  --namespace external-dns --set provider=aws \
  --set txtOwnerId=<your-txt-owner-id> \
  --set domainFilters[0]=<your-domain>
```

The catch is you need someone in charge of Kubernetes, knowledgeable and available set up these controllers, typically a platform engineer.

At small scale, you may only need a single load balancer and DNS record, the effort and complexity make it much like building a factory that ever makes a handful of products.

It's easier and simpler to just write the Terraform code instead of deploying these controllers, building the product instead of the whole factory, to keep that analogy.

Yes, the developers may need to learn enough Terraform to create these things themselves, but they similarly need to learn the Kubernetes YAML definitions and annotations. I'd actually argue Terraform is actually nicer to develop with because you get immediate feedback when you run `terraform validate` or `terraform plan` after writing the code.

In case of Kubernetes failures, let's say you did a typo when writing the annotation and don't get the results you need. You then need to have a look for the logs of the respective controller to look for errors about your change, or maybe raise a ticket to the platform engineer if you lack permissions.

On the ECS world, you'd immediately get an error from Terraform before you even attempt to apply the code changes.

## Final words

Don't get me wrong, Kubernetes makes a lot of sense if you're running at scale, and you have a platform team dedicated to running the clusters, it has many usability benefits for managing infrastructure for hundreds of teams.

But it also has a relatively steep fixed cost regardless of the scale you run at, when it comes to complexity and operational overhead.

I personally find it overkill to use Kubernetes for a small shop of only a handful of developers lacking the skills to operate it. For such situations I recommend to start with ECS and use it until you run into its limits.

To paraphrase someone who commented on my previous blog, start with ECS and only adopt Kubernetes if you have a real need for its advanced features, not just because it looks better on your CV.

That's it for now,

- Cristian

P.S. if you have thoughts about this, join the conversations on [Reddit](https://www.reddit.com/r/aws/comments/14ausdt/why_kubernetes_wasnt_a_good_fit_for_us/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us) and [HackerNews](https://news.ycombinator.com/item?id=36354967&utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us).

P.P.S If you liked this piece and want to get more of this you can sign up or see previous previous [posts](https://leanercloud.beehiiv.com/), check out my **[YouTube channel](https://www.youtube.com/@Leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us)****,** [**Podcast**](https://anchor.fm/leanercloud?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us)**,** [website](https://leanercloud.com/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us), follow me on [Twitter](https://twitter.com/magheru_san?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us) or connect with me on [LinkedIn](https://www.linkedin.com/in/cristimagherusan/?utm_source=leanercloud.beehiiv.com&utm_medium=referral&utm_campaign=why-kubernetes-wasn-t-a-good-fit-for-us).

#### Keep reading

[![Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks ](/images/blog/leanercloud-content/kubernetes-wasnt-good-fit-us/image-4.png)

## Adopting the ONCE model for all my CLI FinOps tools and Terraform building blocks


## Current OpenTofu contributors vs. pledged FTEs

OpenTofu Github stats half a year after the fork.


## Progress report for the first week after forking ec2instances.info


View more
