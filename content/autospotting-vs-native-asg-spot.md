---
title: "AutoSpotting vs Native ASG Spot"
seo_title: "AutoSpotting vs Native AWS Auto Scaling Group Spot Policy"
description: "The native AWS Auto Scaling group Spot integration has no on-demand fallback. See how AutoSpotting adds failover, tag-based setup, and org-wide rollout."
layout: "landing"
faq_data: "autospotting_vs_native_asg_faq"
url: "/autospotting-vs-native-asg-spot/"
---

{{< hero
    headline="AutoSpotting vs native AutoScaling group Spot"
    sub_headline="Both run Spot inside EC2 AutoScaling groups. The native mixed instances policy is built into AWS and free; AutoSpotting adds the parts it leaves out.<br><br>The big one: automatic on-demand fallback, so a group keeps full capacity when Spot runs short instead of silently running fewer instances."
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
    <h2>Two ways to put Spot in an AutoScaling group</h2>
    <p>AWS Auto Scaling groups can run Spot natively through a mixed instances policy on a launch template. You define the on-demand and Spot split, list the compatible instance types, pick an allocation strategy, and the group launches Spot capacity for you. It is a built-in AWS feature at no extra charge, and for teams that are happy to maintain that configuration per group it works well.</p>
    <p>AutoSpotting takes a different route to the same goal. You add a tag to an AutoScaling group you already run, and it converts the group's capacity to Spot without any launch-template changes. The reason to reach for it over the native integration comes down to four differences: automatic on-demand fallback, no launch-template rewrite, fleet-wide rollout, and automated instance-type selection.</p>
  </div>
</section>

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Native ASG Spot vs AutoSpotting</h2>
    </div>
    <div class="overflow-x-auto hidden md:block bg-white rounded-xl border border-gray-200 shadow-sm">
      <table class="w-full min-w-[640px] text-left border-collapse text-sm">
        <thead>
          <tr>
            <th class="p-3 border-b border-gray-200 font-medium text-gray-500 align-bottom"></th>
            <th class="p-3 border-b border-gray-200 font-bold text-gray-900 align-bottom">Native ASG Spot</th>
            <th class="p-3 border-b-2 border-primary-600 font-bold text-primary-800 align-bottom bg-primary-50 rounded-t-lg">AutoSpotting</th>
          </tr>
        </thead>
        <tbody class="text-gray-600">
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Setup</td>
            <td class="p-3 border-b border-gray-100">Convert each group to a launch template with a mixed instances policy</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Add a tag, no config changes</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Instance-type selection</td>
            <td class="p-3 border-b border-gray-100">You maintain per-group type lists</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automated, based on your existing type</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Failover to on-demand</td>
            <td class="p-3 border-b border-gray-100">No automatic fallback once all configured Spot pools are exhausted; runs reduced capacity</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automatic, and back to Spot when it recovers</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Rollout at scale</td>
            <td class="p-3 border-b border-gray-100">Group by group</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Whole account or AWS Org, opt-out mode</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Reverting</td>
            <td class="p-3 border-b border-gray-100">Edit the group back off the mixed instances policy</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Remove the tag to revert</td>
          </tr>
          <tr>
            <td class="p-3 font-medium text-gray-900">Cost</td>
            <td class="p-3">Free, built into AWS</td>
            <td class="p-3 bg-primary-50 rounded-b-lg font-semibold text-primary-800">Free open source, or 10% of savings for the commercial edition</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="md:hidden space-y-4 max-w-screen-md mx-auto">
      <div class="bg-white border border-gray-200 rounded-lg p-5">
        <h3 class="font-bold text-gray-900 mb-3">Native ASG Spot</h3>
        <dl class="divide-y divide-gray-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-gray-800">Convert each group to a launch template with a mixed instances policy</dd></div>
          <div class="py-2"><dt class="text-gray-500">Instance-type selection</dt><dd class="text-gray-800">You maintain per-group type lists</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-800">No automatic fallback once all configured Spot pools are exhausted; runs reduced capacity</dd></div>
          <div class="py-2"><dt class="text-gray-500">Rollout at scale</dt><dd class="text-gray-800">Group by group</dd></div>
          <div class="py-2"><dt class="text-gray-500">Reverting</dt><dd class="text-gray-800">Edit the group back off the mixed instances policy</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-gray-800">Free, built into AWS</dd></div>
        </dl>
      </div>
      <div class="border-2 border-primary-600 bg-primary-50 rounded-lg p-5">
        <h3 class="font-bold text-primary-800 mb-3">AutoSpotting</h3>
        <dl class="divide-y divide-primary-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-primary-800 font-semibold">Add a tag, no config changes</dd></div>
          <div class="py-2"><dt class="text-gray-500">Instance-type selection</dt><dd class="text-gray-900 font-medium">Automated, based on your existing type</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-900 font-medium">Automatic, and back to Spot when it recovers</dd></div>
          <div class="py-2"><dt class="text-gray-500">Rollout at scale</dt><dd class="text-gray-900 font-medium">Whole account or AWS Org, opt-out mode</dd></div>
          <div class="py-2"><dt class="text-gray-500">Reverting</dt><dd class="text-gray-900 font-medium">Remove the tag to revert</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-primary-800 font-semibold">Free open source, or 10% of savings for the commercial edition</dd></div>
        </dl>
      </div>
    </div>
  </div>
</section>

<section class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-16 lg:px-6 prose prose-lg">
    <h2>The difference that bites at the worst time: on-demand fallback</h2>
    <p>This is the caveat few teams know about until it hits them. The native mixed instances policy has no automatic fallback to on-demand once Spot is exhausted across all of the instance types you configured. When that happens, the group does not launch on-demand to make up the shortfall. It simply launches fewer instances and runs at reduced capacity until Spot frees up.</p>
    <p>Spot shortages tend to arrive exactly when you can least afford reduced capacity, such as the end-of-year holiday season, when many workloads peak and Spot pools tighten at the same time. AutoSpotting handles this differently: when Spot capacity runs short across all compatible pools, it launches on-demand instances so the group keeps its full capacity, and it moves back to Spot once the market recovers. You can also configure a minimum number of on-demand instances per group.</p>
    <h2>No launch-template rewrite</h2>
    <p>To run Spot natively you convert each group to a launch template with a mixed instances policy. On a large or legacy estate that is real migration work, group by group, with the risk that comes with editing production launch configuration. AutoSpotting reads the configuration you already have and needs no launch-template changes. You add a tag such as <code>spot-enabled=true</code> and it takes over from there.</p>
    <h2>Fleet-wide, opt-out rollout</h2>
    <p>Because the native integration is configured per group, adoption is a group-by-group project. AutoSpotting can run in opt-out mode across a whole account or an AWS Organization, so new groups are covered by default and you exclude the ones you want to keep on pure on-demand. That turns Spot adoption from a per-group task into a fleet-wide default.</p>
    <h2>Automated instance-type selection</h2>
    <p>The native policy asks you to list compatible instance types for each group and keep those lists current as your workloads and the instance catalog change. AutoSpotting selects compatible types automatically based on the on-demand type the group already runs, and diversifies across them to spread interruption risk, so there is no per-group list to maintain.</p>
    <h2>When native ASG Spot is enough</h2>
    <p>None of this makes the native integration a wrong choice. It is free, fully AWS-supported, and a good fit when you have a small number of groups, you are comfortable maintaining launch templates and instance-type lists, and your workloads can tolerate running at reduced capacity during a Spot shortage. AutoSpotting earns its keep when you want automatic on-demand fallback, tag-based setup with no re-architecting, and rollout across a whole fleet. Both keep your compute inside your own AWS account.</p>
  </div>
</section>

{{< faq data="autospotting_vs_native_asg_faq" />}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/autospotting-vs-spot-io/">AutoSpotting vs Spot.io and commercial Spot managers</a></li>
      <li><a class="hover:underline" href="/autospotting-vs-karpenter/">AutoSpotting vs Karpenter for Spot on Kubernetes</a></li>
      <li><a class="hover:underline" href="/spot-vs-on-demand/">Spot vs on-demand: when each one makes sense</a></li>
      <li><a class="hover:underline" href="/spot-instance-pricing/">How AWS Spot instance pricing works</a></li>
      <li><a class="hover:underline" href="/#compare">Three ways to run Spot, compared</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Add on-demand fallback to your Spot groups"
    description="Tag your existing AutoScaling groups and let AutoSpotting run Spot with automatic on-demand fallback, entirely inside your own AWS account."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
