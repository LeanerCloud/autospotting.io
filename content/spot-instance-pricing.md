---
title: "AWS Spot Instance Pricing"
seo_title: "AWS Spot Instance Pricing: A Practical Guide"
description: "How AWS EC2 Spot instance pricing works, why Spot runs 60-90% below On-Demand, and how AutoSpotting captures those savings in your Auto Scaling groups."
layout: "landing"
faq_data: "spot_pricing_faq"
url: "/spot-instance-pricing/"
---

{{< hero
    headline="AWS Spot Instance Pricing"
    sub_headline="Spot instances run on spare EC2 capacity at 60-90% below On-Demand. Here is how AWS sets Spot prices, why running Spot safely by hand is hard, and how AutoSpotting captures the savings inside your existing Auto Scaling groups.<br><br>• Prices set by AWS per instance type and Availability Zone<br>• Reclaimed on a two-minute notice, with automatic on-demand failover<br>• No re-architecting: add a tag to your Auto Scaling groups"
    primary_button_text="Estimate your savings"
    primary_button_url="https://bit.ly/LCSavingsCalculator"
    secondary_button_text="Install from AWS Marketplace"
    secondary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    gradient-from="#1e40af"
    gradient-to="#7c3aed"
    gradient-angle="135"
>}}

<section id="how-pricing-works" class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md">
      <h2 class="mb-6 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900 text-center">How AWS EC2 Spot pricing works</h2>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">Spot instances are spare EC2 capacity that AWS offers at a large discount, typically 60-90% below the On-Demand rate for the same instance type. You run the exact same instances. What differs is the price, and the fact that AWS can reclaim the capacity when it needs it back.</p>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">AWS sets the Spot price for each instance type in each Availability Zone, based on long-term supply and demand for spare capacity. Since 2017 those prices adjust gradually instead of spiking through a bidding war, so there is no bid to set or manage. You pay whatever the Spot price is while your instance runs, billed by the second like On-Demand.</p>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">Because pricing is per instance type, per region, and per Availability Zone, the same workload can cost noticeably more or less depending on where it runs and which instance types it can use. Spreading across more instance types and zones generally means lower prices and fewer interruptions.</p>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">The catch is interruption. When EC2 needs the capacity back, it reclaims your instance after a two-minute warning. That is the whole trade-off: everything hard about Spot comes from handling that two-minute notice well.</p>
      <p class="font-light text-gray-500 sm:text-lg">You can set a maximum price, but it defaults to the On-Demand rate and does not protect your instance. Reclamation is driven by available capacity, not by price, so bidding higher, even above the On-Demand rate, does not reduce your chance of being terminated. What actually lowers interruptions is choosing instance types with more spare capacity and spreading across several of them. To compare how often each instance type is interrupted, check the <a href="https://aws.amazon.com/ec2/spot/instance-advisor/" target="_blank" rel="noopener" class="text-primary-700 font-medium hover:underline">AWS Spot Instance Advisor</a>.</p>
    </div>
  </div>
</section>

<section id="why-hard" class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md">
      <h2 class="mb-6 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900 text-center">Why Spot is hard to run safely by hand</h2>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">Spot is cheap, but using it safely takes real engineering. To keep capacity when instances are reclaimed, you have to diversify across many compatible instance types and Availability Zones, so a shortage in one pool does not take down your service. You also have to drain load balancer connections before an instance goes away, and fall back to On-Demand when Spot capacity runs out across every pool you configured, then move back to Spot once it recovers.</p>
      <p class="font-light text-gray-500 sm:text-lg">The usual answers each have a catch. The native AWS tooling can do parts of this, but it makes you convert every Auto Scaling group to a launch template with a mixed instances policy and maintain per-group instance-type lists. Commercial Spot managers automate it but run your infrastructure through their SaaS. Reserved Instances and Savings Plans lower the price a different way, by committing your spend for one or three years.</p>
    </div>
  </div>
</section>

<section id="autospotting" class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md">
      <h2 class="mb-6 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900 text-center">How AutoSpotting captures Spot savings automatically</h2>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">AutoSpotting gives you Spot pricing without the manual work. It runs as Lambda functions inside your own AWS account and converts your existing Auto Scaling groups to Spot when you add a single tag. No launch template changes, no re-architecting, no SaaS backend.</p>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">It picks compatible instance types based on the On-Demand instance you already run and diversifies across them. When it launches a new instance, it does not simply take the cheapest option. It applies a prioritization order that weighs availability, price, and instance generation together, favoring the most available of the recent instance types. That keeps performance and availability high while selecting types with a lower likelihood of termination, which reduces interruptions.</p>
      <p class="mb-4 font-light text-gray-500 sm:text-lg">AutoSpotting also drains load balancer traffic before an instance is reclaimed, and fails over to On-Demand automatically when Spot capacity is short, returning to Spot once the market recovers. Remove the tag and the group reverts to plain On-Demand.</p>
      <p class="font-light text-gray-500 sm:text-lg">See how AutoSpotting <a href="/#compare" class="text-primary-700 font-medium hover:underline">compares to the native and SaaS alternatives</a> on the home page, or read the <a href="/#faq" class="text-primary-700 font-medium hover:underline">full AutoSpotting FAQ</a>.</p>
    </div>
  </div>
</section>

<section id="estimate" class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Estimate your Spot savings</h2>
      <p class="mb-6 font-light text-gray-500 sm:text-lg">Prices are dynamic and per-region, so the best number is your own. The free Savings Estimator reads your AWS footprint and can generate an AutoSpotting configuration in one click. Even a relatively small footprint often nets over $1,000/month.</p>
      <div class="flex flex-wrap justify-center gap-4">
        <a href="https://bit.ly/LCSavingsCalculator" target="_blank" rel="noopener" class="inline-block px-6 py-3 text-white bg-primary-700 hover:bg-primary-800 font-medium rounded-lg transition-colors">Estimate your savings</a>
        <a href="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6" target="_blank" rel="noopener" class="inline-block px-6 py-3 text-primary-700 bg-primary-50 hover:bg-primary-100 border border-primary-200 font-medium rounded-lg transition-colors">Install from AWS Marketplace</a>
      </div>
    </div>
  </div>
</section>

{{< faq data="spot_pricing_faq" />}}

{{< cta
    title="Start capturing Spot pricing today"
    description="Add a tag to your Auto Scaling groups and let AutoSpotting handle diversification, draining, and on-demand failover."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
