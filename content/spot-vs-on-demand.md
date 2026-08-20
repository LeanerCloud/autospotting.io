---
title: "Spot vs On-Demand"
seo_title: "EC2 Spot vs On-Demand: When Each One Makes Sense on AWS"
description: "EC2 Spot costs 60-90% less than on-demand but can be reclaimed on a two-minute notice. See which workloads fit Spot and how to run it safely with fallback."
layout: "landing"
url: "/spot-vs-on-demand/"
---

{{< hero
    headline="EC2 Spot vs On-Demand: when each one makes sense"
    sub_headline="Spot capacity is the same hardware as on-demand, at 60-90% off. The catch is that AWS can reclaim it on a two-minute notice.<br><br>This page covers where that trade-off pays off, which workloads belong on Spot, and how AutoSpotting runs Spot with automatic on-demand fallback."
    primary_button_text="Estimate your savings"
    primary_button_url="https://bit.ly/LCSavingsCalculator"
    secondary_button_text="Install from AWS Marketplace"
    secondary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    gradient-from="#1e40af"
    gradient-to="#7c3aed"
    gradient-angle="135"
>}}

<section class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-16 lg:px-6 prose prose-lg">
    <h2>The price gap</h2>
    <p>On-demand instances have a fixed, published hourly price and no interruption risk. You pay full rate for the guarantee that the instance keeps running until you stop it. Spot instances draw from the same EC2 capacity pools, but AWS sells the spare capacity at a discount that typically lands in the 60-90% range for Spot-compatible workloads. The exact discount depends on the region, the instance type, and current capacity in each pool.</p>
    <p>That gap is large enough to change how you budget. A single AutoScaling group of about a dozen c6i.8xlarge instances has saved one AutoSpotting customer roughly $7.5k per month. Across its user base since 2016, AutoSpotting has an estimated $100M+ in delivered savings. The savings are real and repeatable; the reason not everyone runs everything on Spot is the interruption trade-off.</p>

    <h2>The trade-off: two-minute reclamation</h2>
    <p>When AWS needs Spot capacity back, it sends a Spot interruption notice and reclaims the instance two minutes later. Reclamation is driven by capacity in the specific instance pool, not by price. Setting a higher maximum price does not prevent it. If the pool you are drawing from gets tight, your instance goes away regardless of what you were willing to pay.</p>
    <p>So the real question for any workload is: what happens when an instance disappears with two minutes of warning? If the answer is "another instance picks up the work and nothing user-visible breaks," the workload is a good Spot candidate. If the answer is "we lose state or drop a transaction," it belongs on on-demand, or it needs on-demand fallback underneath it.</p>

    <h2>Which workloads suit Spot</h2>
    <p>Good fits are workloads that are already built to tolerate an instance going away:</p>
    <ul>
      <li>Stateless web and API tiers behind a load balancer, where a healthy instance absorbs the traffic.</li>
      <li>Containerized services on ECS or EKS, where the scheduler reschedules tasks and pods onto remaining capacity.</li>
      <li>Batch, data processing, CI runners, and rendering, where work is retried or resumed.</li>
      <li>Fleets that scale up and down daily, which Reserved Instances and Savings Plans cover poorly because the baseline keeps moving.</li>
    </ul>
    <p>Workloads to keep on on-demand, or to protect with fallback: single-instance stateful services with no replica, licensed software pinned to one host, and anything where a two-minute drain is not enough to finish in-flight work cleanly.</p>
    <p>Teams that already practice chaos engineering tend to have the easiest time here. If your systems are built to survive a node failing at any moment, Spot is close to free savings, because the failure mode you are protecting against is the one Spot introduces.</p>

    <h2>You do not have to choose one</h2>
    <p>The practical answer for most fleets is a mix: run on Spot when capacity is available, fall back to on-demand when it is not. That is what AutoSpotting does inside your existing AutoScaling groups. It diversifies across compatible instance types to spread interruption risk, drains connections before an instance is reclaimed, and when Spot capacity runs out across all compatible pools it launches on-demand instances so the group keeps its capacity. When Spot recovers, it moves back.</p>
    <p>This is where AutoSpotting differs from the native Auto Scaling group Spot integration, a caveat few teams know about: the native integration does not fall back to on-demand when Spot runs out across all of your configured instance types. It simply launches fewer instances, so you silently run at reduced capacity until Spot frees up, often exactly when demand is highest, such as the end-of-year holiday season.</p>
    <p>Because it works from a tag on your existing groups and runs entirely in your own AWS account, you can turn it on for a group, measure the savings, and remove the tag to revert to pure on-demand at any time. There is no re-architecting and no cross-account access to grant, which is the design AutoSpotting has kept since it was first built in 2015-2016.</p>
  </div>
</section>

{{< faq >}}
{
  "title": "Spot vs on-demand FAQ",
  "questions": [
    {
      "question": "Is Spot always cheaper than on-demand?",
      "answer": "Spot is discounted from the on-demand rate for the same instance type, typically by 60-90% for Spot-compatible workloads. The discount varies by region, instance type, and current capacity, but Spot prices are set below on-demand for the same hardware."
    },
    {
      "question": "Does a higher maximum price stop my Spot instance from being interrupted?",
      "answer": "No. Interruptions are driven by capacity in the specific instance pool, not by your bid. A higher maximum price does not prevent capacity-based reclamation. Diversifying across instance types and falling back to on-demand are the ways to stay resilient."
    },
    {
      "question": "How much can I save by moving a workload to Spot?",
      "answer": "For Spot-compatible workloads the range is usually 60-90%. Even a relatively small AWS footprint often nets over $1,000 per month. You can estimate your own numbers with the free savings calculator before changing anything."
    },
    {
      "question": "What if Spot capacity runs out?",
      "answer": "AutoSpotting falls back to on-demand instances when Spot capacity is unavailable across all compatible types, so the AutoScaling group keeps its capacity. It returns to Spot once the market recovers, and you can configure a minimum number of on-demand instances per group."
    }
  ]
}
{{< /faq >}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/spot-interruption-handling/">How EC2 Spot interruptions work, and how to handle them</a></li>
      <li><a class="hover:underline" href="/spot-for-kubernetes-ecs/">Running Spot on Kubernetes (EKS) and ECS</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Run Spot safely, with on-demand fallback"
    description="Add a tag to your existing AutoScaling groups and start saving 60-90%, entirely inside your own AWS account."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
