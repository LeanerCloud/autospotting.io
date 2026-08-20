---
title: "Spot Interruption Handling"
seo_title: "EC2 Spot Interruption Handling: How to Run Spot Safely"
description: "EC2 Spot interruptions give a two-minute notice, and a higher max price won't stop them. Pick low-interruption types and fail over to on-demand automatically."
layout: "landing"
faq_data: "spot_interruption_handling_faq"
url: "/spot-interruption-handling/"
---

{{< hero
    headline="How EC2 Spot interruptions work, and how to handle them"
    sub_headline="AWS can reclaim a Spot instance on a two-minute notice when it needs the capacity back. Handling that well is the difference between quiet 60-90% savings and dropped connections.<br><br>Here is how interruptions actually work, and how AutoSpotting absorbs them."
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
    <h2>The two-minute notice</h2>
    <p>When AWS needs Spot capacity back, EC2 issues a Spot interruption notice through instance metadata and an EventBridge event, then reclaims the instance about two minutes later. Two minutes is enough time to stop accepting new work, finish or check-point what is in flight, and drain the instance out of its load balancer, provided something is listening for the notice and acting on it.</p>
    <p>The important part is what triggers a reclamation. It is capacity in the specific instance pool, a combination of instance type and Availability Zone, not price. A common misconception is that setting a higher maximum price keeps an instance safe. It does not. If the pool runs short, AWS reclaims the instance no matter what maximum price you set. Price protects you from paying more; it does not protect you from capacity-based interruption.</p>

    <h2>Choose instance types that get interrupted less</h2>
    <p>Not all Spot pools are equally volatile. Some instance types in some regions are reclaimed rarely; others churn often. AWS publishes the historical interruption frequency for every type in the <a href="https://aws.amazon.com/ec2/spot/instance-advisor/" target="_blank" rel="noopener">Spot Instance Advisor</a>, so you can prefer pools with a low interruption rate before you deploy.</p>
    <p>The single most effective tactic is diversification. If a group can run on many compatible instance types instead of one, a shortage in any single pool only affects a slice of the fleet, and replacements come from pools that still have capacity. A group pinned to one instance type is exposed to that one pool's volatility.</p>
    <p>AutoSpotting automates this selection. It looks at your existing on-demand instance and picks compatible Spot types by weighing availability, price, and instance generation together, favoring the most-available recent-generation types. Newer generations tend to have deeper, less contended capacity, so leaning toward them reduces how often the fleet is interrupted in the first place.</p>

    <h2>Drain before the instance goes away</h2>
    <p>Catching the interruption notice is only half the job. The instance also has to leave its target group cleanly, and this is where a real bug surfaced in practice. An AutoSpotting user running ECS reported dropped connections and 5xx errors during Spot terminations. The cause was that ECS was slow to start draining, sometimes beginning load-balancer deregistration only a few seconds before the instance shut down, and sometimes after it was already gone.</p>
    <p>The fix was to deregister earlier and not wait for the slower path. AutoSpotting now issues the deregistration API calls in less than 10 seconds after the Spot termination event fires, so connections drain while the two-minute window is still open. It extends the same instance and task draining to ECS instances and their target groups. If you run ECS with Spot instances, this draining behavior is worth having, because the failure it prevents is silent until a customer hits a 5xx.</p>

    <h2>Fail over to on-demand automatically</h2>
    <p>Even with good type selection and clean draining, capacity can run short across every compatible pool at once. This happens most during high-demand periods, the classic example being the year-end and Black Friday stretch when Spot capacity tightens broadly. A group restricted to Spot can be left short of capacity exactly when you need it most.</p>
    <p>AutoSpotting handles that by launching on-demand instances as fallback when Spot is unavailable across all compatible types, so the AutoScaling group keeps running at its target size. As soon as a Spot interruption event fires, it also launches replacement Spot instances immediately, with diversified failover to on-demand behind them. When the Spot market recovers, it moves the group back onto Spot. You can also configure a minimum number of on-demand instances per group if you want a permanent baseline that never rides on Spot.</p>
  </div>
</section>

{{< faq data="spot_interruption_handling_faq" />}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/spot-vs-on-demand/">Spot vs on-demand: when each one makes sense</a></li>
      <li><a class="hover:underline" href="/spot-for-kubernetes-ecs/">Running Spot on Kubernetes (EKS) and ECS</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Absorb interruptions instead of chasing them"
    description="AutoSpotting diversifies instance types, drains load balancers, and fails over to on-demand, from a tag on your existing AutoScaling groups."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
