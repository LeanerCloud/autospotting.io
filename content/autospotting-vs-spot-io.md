---
title: "AutoSpotting vs Spot.io"
seo_title: "AutoSpotting vs Spot.io and Commercial Spot Managers"
description: "AutoSpotting and commercial Spot managers like Spot.io both automate Spot. Compare the models: your own AWS account and an open-source core, or a SaaS backend."
layout: "landing"
faq_data: "autospotting_vs_spot_io_faq"
url: "/autospotting-vs-spot-io/"
---

{{< hero
    headline="AutoSpotting vs commercial Spot managers"
    sub_headline="Commercial Spot managers such as Spot.io automate Spot well. The real choice between them and AutoSpotting is about architecture and model, not whether Spot gets automated.<br><br>AutoSpotting runs inside your own AWS account, with no SaaS backend, no cross-account access, and an open-source core."
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
    <h2>What a commercial Spot manager does well</h2>
    <p>Commercial Spot managers such as Spot.io are mature products. They automate the hard parts of running Spot: selecting and diversifying instance types, replacing capacity when it is reclaimed, falling back to on-demand, and presenting a dashboard over your fleet. If your priority is a managed service with a vendor operating the automation for you, that model is a reasonable fit, and we will not pretend otherwise on this page.</p>
    <p>AutoSpotting automates the same core job, converting AutoScaling group capacity to Spot with on-demand fallback and instance-type diversification. Where the two differ is the deployment model, so the honest comparison is on architecture, ownership, and pricing model rather than on a feature checklist. For current terms and capabilities of any specific vendor, check their own site directly; we do not restate other companies' features or pricing here.</p>
    <h2>The architectural difference: your account vs a SaaS backend</h2>
    <p>A commercial SaaS Spot manager runs a backend outside your account and connects to your AWS environment with cross-account access to manage capacity on your behalf. That is how the managed model works, and for some teams the convenience is worth it.</p>
    <p>AutoSpotting runs entirely inside your own AWS account, as Lambda functions, with no SaaS backend and no cross-account access to grant. Nothing about your infrastructure is transmitted to an external service, and it uses minimal IAM permissions. This is the design AutoSpotting has kept since it was first built in 2015-2016, when its author evaluated an early SaaS Spot manager and preferred not to convert everything to a vendor-specific construct or grant a third party standing access to the account.</p>
    <h2>Deployment model and lock-in</h2>
    <p>Commercial managers generally onboard your workloads into their own group constructs. That gives them a consistent surface to manage, and it also means the shape of your setup follows the vendor, which can make leaving harder later. AutoSpotting works from a tag on the AutoScaling groups you already run. There is nothing to migrate into, and removing the tag reverts the group to pure on-demand.</p>
    <h2>Open-source core and pricing model</h2>
    <p>The AutoSpotting core has been open source since 2016 and is available as the Community Edition under the OSL-3.0 license, so you can read the code, build it yourself, and run it at no cost. The commercial edition adds tested binaries, support, and the latest enhancements, priced at 10% of the savings it generates on your AWS bill, billed through the AWS Marketplace, with a free tier for small installations. It is usage-based: you pay a share of realized savings rather than a seat or platform fee, and there is no long-term commitment.</p>
  </div>
</section>

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Commercial Spot manager vs AutoSpotting</h2>
      <p class="font-light text-gray-500 sm:text-lg">Compared on model and architecture, the parts that do not change from one release to the next.</p>
    </div>
    <div class="overflow-x-auto hidden md:block bg-white rounded-xl border border-gray-200 shadow-sm">
      <table class="w-full min-w-[640px] text-left border-collapse text-sm">
        <thead>
          <tr>
            <th class="p-3 border-b border-gray-200 font-medium text-gray-500 align-bottom"></th>
            <th class="p-3 border-b border-gray-200 font-bold text-gray-900 align-bottom">Commercial Spot manager</th>
            <th class="p-3 border-b-2 border-primary-600 font-bold text-primary-800 align-bottom bg-primary-50 rounded-t-lg">AutoSpotting</th>
          </tr>
        </thead>
        <tbody class="text-gray-600">
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Setup</td>
            <td class="p-3 border-b border-gray-100">Onboard to their SaaS, adopt their group constructs</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Add a tag, no config changes</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Where it runs, your data</td>
            <td class="p-3 border-b border-gray-100">Their SaaS, with cross-account access</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Your account, no SaaS backend</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Failover to on-demand</td>
            <td class="p-3 border-b border-gray-100">Yes</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automatic, and back to Spot when it recovers</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Source model</td>
            <td class="p-3 border-b border-gray-100">Closed source</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Open-source core (OSL-3.0)</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Lock-in</td>
            <td class="p-3 border-b border-gray-100">Vendor constructs, harder to leave</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">None; remove the tag to revert</td>
          </tr>
          <tr>
            <td class="p-3 font-medium text-gray-900">Cost</td>
            <td class="p-3">A share of savings</td>
            <td class="p-3 bg-primary-50 rounded-b-lg font-semibold text-primary-800">Free open source, or 10% of savings for the commercial edition</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="md:hidden space-y-4 max-w-screen-md mx-auto">
      <div class="bg-white border border-gray-200 rounded-lg p-5">
        <h3 class="font-bold text-gray-900 mb-3">Commercial Spot manager</h3>
        <dl class="divide-y divide-gray-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-gray-800">Onboard to their SaaS, adopt their group constructs</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs, your data</dt><dd class="text-gray-800">Their SaaS, with cross-account access</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-800">Yes</dd></div>
          <div class="py-2"><dt class="text-gray-500">Source model</dt><dd class="text-gray-800">Closed source</dd></div>
          <div class="py-2"><dt class="text-gray-500">Lock-in</dt><dd class="text-gray-800">Vendor constructs, harder to leave</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-gray-800">A share of savings</dd></div>
        </dl>
      </div>
      <div class="border-2 border-primary-600 bg-primary-50 rounded-lg p-5">
        <h3 class="font-bold text-primary-800 mb-3">AutoSpotting</h3>
        <dl class="divide-y divide-primary-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-primary-800 font-semibold">Add a tag, no config changes</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs, your data</dt><dd class="text-primary-800 font-semibold">Your account, no SaaS backend</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-900 font-medium">Automatic, and back to Spot when it recovers</dd></div>
          <div class="py-2"><dt class="text-gray-500">Source model</dt><dd class="text-gray-900 font-medium">Open-source core (OSL-3.0)</dd></div>
          <div class="py-2"><dt class="text-gray-500">Lock-in</dt><dd class="text-gray-900 font-medium">None; remove the tag to revert</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-primary-800 font-semibold">Free open source, or 10% of savings for the commercial edition</dd></div>
        </dl>
      </div>
    </div>
  </div>
</section>

<section class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-16 lg:px-6 prose prose-lg">
    <h2>A note on forks of the open-source core</h2>
    <p>Because AutoSpotting's core has been open source since 2016, some commercial products trace back to that early codebase. Xosphere, for example, is a commercial fork of the original MIT-licensed AutoSpotting. AutoSpotting is the actively developed original: its architecture moved from the initial cron-based model to an event-based one years ago. If you are evaluating a fork, it is worth confirming which architecture it runs today and comparing that against the current AutoSpotting directly, rather than assuming a shared history means shared behavior.</p>
    <h2>Which model fits you</h2>
    <p>If you want a fully managed SaaS and are comfortable with a vendor operating the automation and holding cross-account access, a commercial Spot manager is a reasonable choice, and you should compare its current features and pricing on its own terms. If you want the automation to run inside your own account with no SaaS backend, an open-source core you can inspect, tag-based setup on the groups you already have, and usage-based pricing tied to realized savings, that is what AutoSpotting is built for.</p>
  </div>
</section>

{{< faq data="autospotting_vs_spot_io_faq" />}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/autospotting-vs-native-asg-spot/">AutoSpotting vs native AutoScaling group Spot</a></li>
      <li><a class="hover:underline" href="/autospotting-vs-karpenter/">AutoSpotting vs Karpenter for Spot on Kubernetes</a></li>
      <li><a class="hover:underline" href="/spot-vs-on-demand/">Spot vs on-demand: when each one makes sense</a></li>
      <li><a class="hover:underline" href="/spot-instance-pricing/">How AWS Spot instance pricing works</a></li>
      <li><a class="hover:underline" href="/#compare">Three ways to run Spot, compared</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Run Spot in your own account"
    description="Tag your existing AutoScaling groups and let AutoSpotting handle diversification, draining, and on-demand fallback, with no SaaS backend and no cross-account access."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
