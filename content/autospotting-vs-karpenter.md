---
title: "AutoSpotting vs Karpenter"
seo_title: "AutoSpotting vs Karpenter for EC2 Spot on Kubernetes"
description: "Karpenter provisions Kubernetes nodes and can request Spot. AutoSpotting works at the AutoScaling-group layer with on-demand fallback. See where each fits."
layout: "landing"
faq_data: "autospotting_vs_karpenter_faq"
url: "/autospotting-vs-karpenter/"
---

{{< hero
    headline="AutoSpotting vs Karpenter for Spot on Kubernetes"
    sub_headline="Karpenter is a capable Kubernetes-native node provisioner that can request Spot directly. AutoSpotting works one layer down, at the AutoScaling group.<br><br>They sit at different layers, so this is less about which is better and more about which fits your setup, and when to use both."
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
    <h2>Different layers, different jobs</h2>
    <p>Karpenter is an open-source Kubernetes node provisioner. It watches for pending pods and launches right-sized nodes to fit them, it can request Spot capacity directly, and it consolidates nodes as workloads change. If you want a Kubernetes-native provisioner and you are moving off Cluster Autoscaler, Karpenter is a strong, widely used choice.</p>
    <p>AutoSpotting works one layer down, at the EC2 AutoScaling group. It converts the capacity of groups you already run to Spot, diversifies across compatible instance types, and falls back to on-demand automatically when Spot runs short. It is not a Kubernetes provisioner; it works with any service backed by an AutoScaling group. That includes EKS managed node groups and self-managed Kubernetes node pools, and also ECS, Elastic Beanstalk, and plain AutoScaling groups. So the comparison is not really either-or: the two operate at different layers and often suit different parts of the same estate.</p>
  </div>
</section>

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Karpenter vs AutoSpotting</h2>
    </div>
    <div class="overflow-x-auto hidden md:block bg-white rounded-xl border border-gray-200 shadow-sm">
      <table class="w-full min-w-[640px] text-left border-collapse text-sm">
        <thead>
          <tr>
            <th class="p-3 border-b border-gray-200 font-medium text-gray-500 align-bottom"></th>
            <th class="p-3 border-b border-gray-200 font-bold text-gray-900 align-bottom">Karpenter</th>
            <th class="p-3 border-b-2 border-primary-600 font-bold text-primary-800 align-bottom bg-primary-50 rounded-t-lg">AutoSpotting</th>
          </tr>
        </thead>
        <tbody class="text-gray-600">
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Scope</td>
            <td class="p-3 border-b border-gray-100">Kubernetes nodes (EKS, self-managed)</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Any AutoScaling-group-backed service (EKS, ECS, Beanstalk, plain ASGs)</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Adoption</td>
            <td class="p-3 border-b border-gray-100">Replaces your node provisioner; you define and operate NodePools</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Add a tag to existing groups; no provisioner change</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Spot handling</td>
            <td class="p-3 border-b border-gray-100">Requests Spot capacity directly for pending pods; consolidates nodes</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Converts existing group capacity to Spot, diversified across compatible types</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">On-demand fallback</td>
            <td class="p-3 border-b border-gray-100">Configurable via NodePools and weights</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automatic when Spot is exhausted across compatible pools; returns to Spot on recovery</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Where it runs</td>
            <td class="p-3 border-b border-gray-100">In-cluster controller</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Lambda in your AWS account, no SaaS backend</td>
          </tr>
          <tr>
            <td class="p-3 font-medium text-gray-900">Cost</td>
            <td class="p-3">Open source</td>
            <td class="p-3 bg-primary-50 rounded-b-lg font-semibold text-primary-800">Open-source core, or 10% of savings for the commercial edition</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="md:hidden space-y-4 max-w-screen-md mx-auto">
      <div class="bg-white border border-gray-200 rounded-lg p-5">
        <h3 class="font-bold text-gray-900 mb-3">Karpenter</h3>
        <dl class="divide-y divide-gray-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Scope</dt><dd class="text-gray-800">Kubernetes nodes (EKS, self-managed)</dd></div>
          <div class="py-2"><dt class="text-gray-500">Adoption</dt><dd class="text-gray-800">Replaces your node provisioner; you define and operate NodePools</dd></div>
          <div class="py-2"><dt class="text-gray-500">Spot handling</dt><dd class="text-gray-800">Requests Spot capacity directly for pending pods; consolidates nodes</dd></div>
          <div class="py-2"><dt class="text-gray-500">On-demand fallback</dt><dd class="text-gray-800">Configurable via NodePools and weights</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs</dt><dd class="text-gray-800">In-cluster controller</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-gray-800">Open source</dd></div>
        </dl>
      </div>
      <div class="border-2 border-primary-600 bg-primary-50 rounded-lg p-5">
        <h3 class="font-bold text-primary-800 mb-3">AutoSpotting</h3>
        <dl class="divide-y divide-primary-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Scope</dt><dd class="text-gray-900 font-medium">Any AutoScaling-group-backed service (EKS, ECS, Beanstalk, plain ASGs)</dd></div>
          <div class="py-2"><dt class="text-gray-500">Adoption</dt><dd class="text-primary-800 font-semibold">Add a tag to existing groups; no provisioner change</dd></div>
          <div class="py-2"><dt class="text-gray-500">Spot handling</dt><dd class="text-gray-900 font-medium">Converts existing group capacity to Spot, diversified across compatible types</dd></div>
          <div class="py-2"><dt class="text-gray-500">On-demand fallback</dt><dd class="text-gray-900 font-medium">Automatic when Spot is exhausted across compatible pools; returns to Spot on recovery</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs</dt><dd class="text-primary-800 font-semibold">Lambda in your AWS account, no SaaS backend</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-primary-800 font-semibold">Open-source core, or 10% of savings for the commercial edition</dd></div>
        </dl>
      </div>
    </div>
  </div>
</section>

<section class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-16 lg:px-6 prose prose-lg">
    <h2>Where Karpenter fits</h2>
    <p>Karpenter is the right tool when you want a Kubernetes-native provisioner in charge of your nodes. It shines when you are replacing Cluster Autoscaler, when you want nodes sized and consolidated in response to pending pods, and when your team is ready to define and operate NodePools as part of how the cluster runs. It requests Spot directly, and you can mix Spot and on-demand through NodePool configuration. If Kubernetes is your whole world and you want provisioning to live inside the cluster, Karpenter is a natural home for that.</p>
    <h2>Where AutoSpotting fits</h2>
    <p>AutoSpotting is the right tool when you do not want to replace your node provisioner. If your EKS or self-managed clusters run on managed node groups or self-managed AutoScaling groups that you are happy with, AutoSpotting brings Spot to that existing capacity with automatic on-demand fallback, by adding a tag. There is no new provisioning model to adopt and operate.</p>
    <p>It also fits when Kubernetes is only part of the picture. Because it works at the AutoScaling group layer, the same tag on the same kind of group also covers ECS, Elastic Beanstalk, and plain AutoScaling groups. One team can use one approach for Spot across Kubernetes and non-Kubernetes workloads alike, instead of one tool for clusters and something else for everything else. Everything runs inside your own AWS account, with no SaaS backend and no cross-account access.</p>
    <h2>A note on reduced capacity</h2>
    <p>One behavior worth understanding across all of these options is what happens when Spot runs out. EKS managed node groups running Spot, and the native Auto Scaling group Spot integration in general, do not fall back to on-demand once Spot is exhausted across all configured instance types. They launch fewer nodes, so the cluster silently drops to reduced capacity until Spot frees up, often during high-demand periods such as the end-of-year holiday season. AutoSpotting fails over to on-demand automatically at the AutoScaling group layer, so the group keeps full capacity. With Karpenter, on-demand fallback depends on how you configure your NodePools and weights, so it is a setting to get right rather than an automatic default.</p>
    <h2>Using both</h2>
    <p>These are not mutually exclusive. Karpenter can run the clusters where you want a native provisioner, while AutoSpotting brings Spot with automatic on-demand fallback to the AutoScaling groups behind your other clusters and your non-Kubernetes services. Pick per workload rather than treating it as a single all-or-nothing decision. For a deeper look at Spot on containers, including ECS draining, see our guide to <a href="/spot-for-kubernetes-ecs/">running Spot on Kubernetes and ECS</a>.</p>
  </div>
</section>

{{< faq data="autospotting_vs_karpenter_faq" />}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/spot-for-kubernetes-ecs/">Running Spot on Kubernetes (EKS) and ECS</a></li>
      <li><a class="hover:underline" href="/autospotting-vs-native-asg-spot/">AutoSpotting vs native AutoScaling group Spot</a></li>
      <li><a class="hover:underline" href="/autospotting-vs-spot-io/">AutoSpotting vs Spot.io and commercial Spot managers</a></li>
      <li><a class="hover:underline" href="/spot-vs-on-demand/">Spot vs on-demand: when each one makes sense</a></li>
      <li><a class="hover:underline" href="/spot-instance-pricing/">How AWS Spot instance pricing works</a></li>
      <li><a class="hover:underline" href="/#compare">Three ways to run Spot, compared</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Bring Spot to your existing node groups"
    description="Tag the AutoScaling groups behind your clusters and services, and let AutoSpotting handle diversification, draining, and automatic on-demand fallback."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
