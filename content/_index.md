---
title: "AutoSpotting - Save 60-90% on AWS EC2 Costs"
description: "AutoSpotting automatically converts AutoScaling groups to Spot instances, saving 60-90% on AWS EC2 costs with minimal friction and production-grade reliability"
testimonials:
  - name: "Jan Čurn"
    title: "CEO, Apify"
    company: "Apify"
    company_url: "https://apify.com"
    company_logo: "/images/clients/apify.png"
    avatar: "/images/testimonials/jan-curn.jpg"
    quote: "I think AutoSpotting has by far the best approach to utilizing spot instances that I've seen. With AutoSpotting we're pretty much able to run our whole stateless production workload on spot instances."
  - name: "Andreas Sundström"
    title: "DevOps & Development Director, OhMy"
    company: "OhMy"
    company_url: "https://www.ohmy.no"
    company_logo: "/images/clients/ohmy.png"
    avatar: "/images/testimonials/andreas.jpg"
    quote: "Thanks to AutoSpotting we're saving more than 50% of our server costs on AWS"
  - name: "Falko Zurell"
    title: "Head of Platform Operations, HERE"
    company: "HERE Technologies"
    company_url: "https://www.here.com"
    company_logo: "/images/clients/here.png"
    avatar: "/images/testimonials/falko.png"
    quote: "Just deployed my first installation of the AutoSpotting tool… It took only 3 minutes and 5 clicks and now I'm saving 60% of my EC2 cost with Spot instances automagically. Kudos Sir!"
  - name: "Levi McCormick"
    title: "Lead Cloud Engineer, SPSCommerce"
    company: "SPS Commerce"
    company_url: "https://www.spscommerce.com"
    avatar: "/images/testimonials/levi.jpg"
    quote: "I recently launched Autospotting to help with our deployments that can't use fleets. It's been working really well."
  - name: "Pierre Allétru"
    title: "CEO, postale.io"
    company: "postale.io"
    company_url: "https://postale.io"
    company_logo: "/images/clients/postale.svg"
    avatar: "/images/testimonials/pierre-alletru.jpg"
    quote: "AutoSpotting is a fantastic service that has exceeded our expectations on what we were looking for: easy conversion to spot, fallback to on-demand, and no lock in. It completely crushes its competitors. Cristian also provided excellent support. Thank you for a job well done!"
  - name: "Jacob Cooper"
    title: "CTO, Flip CX"
    company: "Flip CX"
    company_url: "https://flipcx.com"
    company_logo: "/images/clients/flipcx.png"
    quote: "This was amazing technical support - thank you!!! I wish I got this from every provider that I worked with, wow."
  - name: "Klas Wikblad"
    title: "CTO, APPRL"
    company: "APPRL"
    company_url: "https://apprl.com"
    company_logo: "/images/clients/apprl.png"
    avatar: "/images/testimonials/klas-wikblad.jpg"
    quote: "AutoSpotting is such a great product, just incredibly smooth and well working. We did not even notice that it was running for several years."
client_logos:
  - name: "Expedia"
    logo: "/images/clients/expedia.jpg"
  - name: "Samsung"
    logo: "/images/clients/samsung.svg"
  - name: "Qualcomm"
    logo: "/images/clients/qualcomm.png"
  - name: "TED"
    logo: "/images/clients/ted.png"
  - name: "Mozilla"
    logo: "/images/clients/mozilla.svg"
  - name: "UK Department for Work & Pensions"
    logo: "/images/clients/dwp.png"
  - name: "University of Texas"
    logo: "/images/clients/utaustin.png"
  - name: "Telefonica"
    logo: "/images/clients/telefonica.png"
  - name: "Canal+"
    logo: "/images/clients/canalplus.svg"
  - name: "Here Technologies"
    logo: "/images/clients/here.png"
  - name: "Apify"
    logo: "/images/clients/apify.png"
  - name: "OhMy"
    logo: "/images/clients/ohmy.png"
  - name: "Audiomack"
    logo: "/images/clients/audiomack.png"
---

{{< hero
    headline="Save 60-90% on AWS EC2 Costs"
    sub_headline="AutoSpotting runs Spot inside your existing AutoScaling groups, with automatic failover to on-demand.<br><br>• No re-architecting, no commitments, no lock-in<br>• Add a tag, install in minutes<br>• Runs in your account, no SaaS backend"
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="See Pricing"
    secondary_button_url="/#pricing"
    hero_image="/images/savings-before-after.png"
    hero_image_alt="AWS EC2 costs before and after installing AutoSpotting"
    zoom="true"
    gradient-from="#1e40af"
    gradient-to="#7c3aed"
    gradient-angle="135"
>}}

<section id="problem" class="bg-white">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Spot instances are 60-90% cheaper. So why isn't everything on Spot?</h2>
      <p class="mb-4 font-light text-gray-500 sm:text-xl">Because doing it safely by hand is hard. Spot capacity can be reclaimed on a two-minute notice, so you have to diversify across instance types, drain load balancers, and fall back to on-demand when capacity runs out.</p>
      <p class="font-light text-gray-500 sm:text-lg">The usual answers each have a catch: the native AWS tooling makes you re-architect every AutoScaling group, commercial Spot managers put your infrastructure behind their SaaS and lock you in, and Reserved Instances or Savings Plans commit your spend for years. AutoSpotting handles all of it from inside your own account, by adding a tag.</p>
    </div>
  </div>
</section>

<section id="compare" class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-3xl md:text-4xl tracking-tight font-extrabold text-gray-900">Three ways to run Spot. One just adds a tag.</h2>
    </div>
    <div class="overflow-x-auto hidden md:block bg-white rounded-xl border border-gray-200 shadow-sm">
      <table class="w-full min-w-[820px] text-left border-collapse text-sm">
        <thead>
          <tr>
            <th class="p-3 border-b border-gray-200 font-medium text-gray-500 align-bottom"></th>
            <th class="p-3 border-b border-gray-200 font-bold text-gray-900 align-bottom">Native ASG Spot</th>
            <th class="p-3 border-b border-gray-200 font-bold text-gray-900 align-bottom">Commercial Spot manager like Spot.io</th>
            <th class="p-3 border-b-2 border-primary-600 font-bold text-primary-800 align-bottom bg-primary-50 rounded-t-lg">AutoSpotting</th>
          </tr>
        </thead>
        <tbody class="text-gray-600">
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Setup</td>
            <td class="p-3 border-b border-gray-100">Convert each group to launch templates, pick instance types</td>
            <td class="p-3 border-b border-gray-100">Onboard to their SaaS, adopt their group constructs</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Add a tag, no config changes</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Instance-type selection</td>
            <td class="p-3 border-b border-gray-100">You maintain per-group lists</td>
            <td class="p-3 border-b border-gray-100">Automated</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automated, based on your existing type</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Failover to on-demand</td>
            <td class="p-3 border-b border-gray-100">Limited; the group can run short</td>
            <td class="p-3 border-b border-gray-100">Yes</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Automatic, and back to Spot when it recovers</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Where it runs, your data</td>
            <td class="p-3 border-b border-gray-100">AWS-native</td>
            <td class="p-3 border-b border-gray-100">Their SaaS, with cross-account access</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 font-semibold text-primary-800">Your account, no SaaS backend</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Lock-in</td>
            <td class="p-3 border-b border-gray-100">None, but you re-architected</td>
            <td class="p-3 border-b border-gray-100">Vendor constructs, hard to leave</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">None; remove the tag to revert</td>
          </tr>
          <tr>
            <td class="p-3 border-b border-gray-100 font-medium text-gray-900">Rollout at scale</td>
            <td class="p-3 border-b border-gray-100">Group by group</td>
            <td class="p-3 border-b border-gray-100">Per their onboarding</td>
            <td class="p-3 border-b border-gray-100 bg-primary-50 text-gray-900">Whole account or AWS Org, opt-out mode</td>
          </tr>
          <tr>
            <td class="p-3 font-medium text-gray-900">Cost</td>
            <td class="p-3">Free</td>
            <td class="p-3">A share of savings, often up to ~20%</td>
            <td class="p-3 bg-primary-50 rounded-b-lg font-semibold text-primary-800">Free open source, or 10% of savings for the more robust, enhanced commercial version</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="md:hidden space-y-4 max-w-screen-md mx-auto">
      <div class="bg-white border border-gray-200 rounded-lg p-5">
        <h3 class="font-bold text-gray-900 mb-3">Native ASG Spot</h3>
        <dl class="divide-y divide-gray-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-gray-800">Convert each group to launch templates, pick instance types</dd></div>
          <div class="py-2"><dt class="text-gray-500">Instance-type selection</dt><dd class="text-gray-800">You maintain per-group lists</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-800">Limited; the group can run short</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs, your data</dt><dd class="text-gray-800">AWS-native</dd></div>
          <div class="py-2"><dt class="text-gray-500">Lock-in</dt><dd class="text-gray-800">None, but you re-architected</dd></div>
          <div class="py-2"><dt class="text-gray-500">Rollout at scale</dt><dd class="text-gray-800">Group by group</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-gray-800">Free</dd></div>
        </dl>
      </div>
      <div class="bg-white border border-gray-200 rounded-lg p-5">
        <h3 class="font-bold text-gray-900 mb-3">Commercial Spot manager like Spot.io</h3>
        <dl class="divide-y divide-gray-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-gray-800">Onboard to their SaaS, adopt their group constructs</dd></div>
          <div class="py-2"><dt class="text-gray-500">Instance-type selection</dt><dd class="text-gray-800">Automated</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-800">Yes</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs, your data</dt><dd class="text-gray-800">Their SaaS, with cross-account access</dd></div>
          <div class="py-2"><dt class="text-gray-500">Lock-in</dt><dd class="text-gray-800">Vendor constructs, hard to leave</dd></div>
          <div class="py-2"><dt class="text-gray-500">Rollout at scale</dt><dd class="text-gray-800">Per their onboarding</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-gray-800">A share of savings, often up to ~20%</dd></div>
        </dl>
      </div>
      <div class="border-2 border-primary-600 bg-primary-50 rounded-lg p-5">
        <h3 class="font-bold text-primary-800 mb-3">AutoSpotting</h3>
        <dl class="divide-y divide-primary-100 text-sm">
          <div class="py-2"><dt class="text-gray-500">Setup</dt><dd class="text-primary-800 font-semibold">Add a tag, no config changes</dd></div>
          <div class="py-2"><dt class="text-gray-500">Instance-type selection</dt><dd class="text-gray-900 font-medium">Automated, based on your existing type</dd></div>
          <div class="py-2"><dt class="text-gray-500">Failover to on-demand</dt><dd class="text-gray-900 font-medium">Automatic, and back to Spot when it recovers</dd></div>
          <div class="py-2"><dt class="text-gray-500">Where it runs, your data</dt><dd class="text-primary-800 font-semibold">Your account, no SaaS backend</dd></div>
          <div class="py-2"><dt class="text-gray-500">Lock-in</dt><dd class="text-gray-900 font-medium">None; remove the tag to revert</dd></div>
          <div class="py-2"><dt class="text-gray-500">Rollout at scale</dt><dd class="text-gray-900 font-medium">Whole account or AWS Org, opt-out mode</dd></div>
          <div class="py-2"><dt class="text-gray-500">Cost</dt><dd class="text-primary-800 font-semibold">Free open source, or 10% of savings for the more robust, enhanced commercial version</dd></div>
        </dl>
      </div>
    </div>
  </div>
</section>

<div id="features"></div>

{{< features-section
    title="Making Spot instances safe for production"
    description="AutoSpotting has helped thousands of companies and institutions save over $100,000,000 since 2016"
>}}

{{< feature
    title="Production-Ready Reliability"
    description="Automatic failover to on-demand instances ensures your applications stay running even when spot capacity is unavailable."
    icon="shield"
    features="Zero downtime during spot interruptions,Traffic draining for load balancers,Configurable on-demand instance retention,Trusted by major enterprises worldwide"
>}}

{{< feature
    title="Install in Minutes"
    description="Get started immediately with CloudFormation or Terraform. Tag your existing AutoScaling groups with 'spot-enabled=true' - no launch template changes required. An experimental GUI to estimate savings and enable with a click is in progress."
    icon="bolt"
    features="5-minute installation,Works with existing infrastructure,Deploy across multiple AWS accounts with StackSets,Start saving costs today"
>}}

{{< feature
    title="Your Data Stays Private"
    description="Runs entirely within your AWS account as Lambda functions. No SaaS backend, no external data transmission, minimal IAM permissions required."
    icon="lock"
    features="Complete data privacy,Full auditability and control,Industry-standard security practices,No vendor access to your infrastructure"
>}}

{{< feature
    title="Works With Your Stack"
    description="Seamless integration with ECS, EKS, Elastic Beanstalk, and any service backed by AutoScaling groups. Fits naturally into your CI/CD pipelines."
    icon="integration"
    features="Zero vendor lock-in,Easy to suspend or remove anytime,Complements existing AWS services,Multi-account support"
>}}

{{< feature
    title="Minimal Cost to Save Big"
    description="Pay only 10% of the savings generated through your AWS bill. Serverless architecture means negligible runtime costs."
    icon="tag"
    features="No upfront investment,Pay only for results,Free tier for small installations,Cancel anytime with no penalties"
>}}

{{< feature
    title="Backed By Expert Support"
    description="Stable, tested binaries delivered through AWS Marketplace. Get setup assistance and long-term support from the team that built AutoSpotting."
    icon="headset"
    features="Priority support for paying customers,Help with installation and optimization,Regular updates and improvements,Dedicated team with deep AWS expertise"
>}}

{{< feature
    title="Visual Dashboard & Analytics (Experimental)"
    description="An experimental web interface to monitor savings, manage AutoScaling groups, and analyze cost optimization in real time. In progress and off by default for now."
    icon="chart"
    features="Real-time savings tracking,One-click spot enablement for ASGs,Historical cost analysis and reports,Visual savings estimates before enabling"
>}}

{{< /features-section >}}

<section id="scenarios" class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-4xl tracking-tight font-extrabold text-gray-900">Typical Scenarios</h2>
      <p class="mb-5 font-light text-gray-500 sm:text-xl">Where AutoSpotting delivers the most value</p>
    </div>
    <div class="grid md:grid-cols-2 gap-8 max-w-screen-lg mx-auto">
      <div class="bg-white border rounded-lg p-6 shadow-sm">
        <h3 class="text-xl font-bold text-gray-900 mb-2">Daily capacity scaling</h3>
        <p class="text-gray-600">Workloads that scale up and down daily, which can't be effectively covered by Savings Plans or Reserved Instances.</p>
      </div>
      <div class="bg-white border rounded-lg p-6 shadow-sm">
        <h3 class="text-xl font-bold text-gray-900 mb-2">No long-term commitments</h3>
        <p class="text-gray-600">When the multi-year commitments of Reserved Instances and Savings Plans aren't desirable for your business.</p>
      </div>
      <div class="bg-white border rounded-lg p-6 shadow-sm">
        <h3 class="text-xl font-bold text-gray-900 mb-2">No upfront payments</h3>
        <p class="text-gray-600">Achieve the highest possible cost savings without capital-intensive 3-year all-upfront payments.</p>
      </div>
      <div class="bg-white border rounded-lg p-6 shadow-sm">
        <h3 class="text-xl font-bold text-gray-900 mb-2">Easy adoption at scale</h3>
        <p class="text-gray-600">Convert to Spot (and back to on-demand) reliably without per-group configuration changes - ideal for large organizations and legacy environments.</p>
      </div>
    </div>
    <div class="mx-auto max-w-screen-md text-center mt-12">
      <h3 class="mb-2 text-2xl font-bold text-gray-900">Estimate your savings</h3>
      <p class="mb-6 font-light text-gray-500 sm:text-lg">Even a relatively small AWS footprint often nets over $1,000/month. Generate your own estimate with our free tools, which can also produce your AutoSpotting configuration with a single click.</p>
      <div class="flex flex-wrap justify-center gap-4">
        <a href="https://bit.ly/LCSavingsCalculator" target="_blank" rel="noopener" class="inline-block px-6 py-3 text-white bg-primary-700 hover:bg-primary-800 font-medium rounded-lg transition-colors">Savings Calculator</a>
        <a href="https://github.com/LeanerCloud/savings-estimator" target="_blank" rel="noopener" class="inline-block px-6 py-3 text-primary-700 bg-primary-50 hover:bg-primary-100 border border-primary-200 font-medium rounded-lg transition-colors">Savings Simulator App</a>
      </div>
    </div>
  </div>
</section>

<section id="demo" class="bg-white dark:bg-gray-900">
  <div class="py-8 px-4 mx-auto max-w-screen-xl lg:py-16 lg:px-6">
    <div class="mx-auto max-w-screen-md text-center mb-8 lg:mb-12">
      <h2 class="mb-4 text-4xl tracking-tight font-extrabold text-gray-900 dark:text-white">See AutoSpotting in Action</h2>
      <p class="mb-5 font-light text-gray-500 sm:text-xl dark:text-gray-400">A quick introduction, and a real installation that reached 85% savings within minutes</p>
    </div>
    <div class="grid md:grid-cols-2 gap-8 max-w-screen-lg mx-auto">
      <div>
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-3 text-center">How AutoSpotting works</h3>
        {{< youtube foobAmWpexI >}}
      </div>
      <div>
        <h3 class="text-lg font-semibold text-gray-900 dark:text-white mb-3 text-center">Installed in minutes, 85% savings</h3>
        {{< youtube yGyh6cZESPA >}}
      </div>
    </div>
  </div>
</section>

<div id="testimonials"></div>

{{< testimonials
    title="Trusted by Leading Organizations"
    description="See how companies worldwide are saving millions with AutoSpotting"
    animate="true"
    background-color="#f8fafc"
>}}

<div id="users"></div>

{{< client-logos title="Trusted by teams at leading companies worldwide" animate="true" >}}

<div id="pricing"></div>

{{< pricing-table-2 >}}
{
  "title": "Flexible Pricing Options",
  "description": "Choose the plan that best fits your needs",
  "plans": [
    {
      "name": "Community Edition",
      "description": "Free and open-source",
      "features": [
        "Open Source (OSL-3.0 license)",
        "Community support via GitHub",
        "Build from source code",
        "Unlimited customization",
        "No savings limits",
        "Perfect for evaluation",
        "Must share any software changes with the community",
        "The commercial version has had many more improvements lately"
      ],
      "button": {
        "text": "Get Started on GitHub",
        "url": "https://github.com/AutoSpotting/AutoSpotting"
      },
      "featured": false
    },
    {
      "name": "Pay as you go",
      "description": "10% of monthly savings",
      "features": [
        "Installs in minutes via CloudFormation or Terraform",
        "Enterprise support included",
        "Charged on your AWS bill",
        "Pay as you go - no upfront costs",
        "Perpetual free tier for small installations",
        "Tested and stable binaries",
        "AWS Marketplace billing",
        "Cancel anytime"
      ],
      "button": {
        "text": "Install from AWS Marketplace",
        "url": "https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
      },
      "featured": true
    },
    {
      "name": "Enterprise",
      "description": "Custom flat pricing based on your footprint",
      "features": [
        "Predictable flat monthly fee",
        "Based on your AWS footprint",
        "Priority enterprise support",
        "Custom SLAs available",
        "Dedicated account management",
        "Training and onboarding included",
        "Volume discounts",
        "Annual billing options",
        "Customizations and whitelabeling available as additional service"
      ],
      "button": {
        "text": "Contact Sales",
        "url": "mailto:sales@autospotting.io?subject=Enterprise%20Pricing%20Inquiry&body=Hi,%0D%0A%0D%0AI'm%20interested%20in%20learning%20more%20about%20AutoSpotting's%20Enterprise%20tier%20with%20flat%20pricing.%0D%0A%0D%0AMy%20AWS%20footprint:%0D%0A-%20Number%20of%20instances:%0D%0A-%20Monthly%20AWS%20spend:%0D%0A-%20Regions:%0D%0A%0D%0APlease%20contact%20me%20to%20discuss%20pricing%20and%20features.%0D%0A%0D%0AThank%20you"
      },
      "featured": false
    }
  ]
}
{{< /pricing-table-2 >}}

{{< faq >}}
{
  "title": "Frequently Asked Questions",
  "description": "Everything you need to know about AutoSpotting",
  "questions": [
    {
      "question": "How is AutoSpotting different from the native AWS Auto Scaling Spot integration?",
      "answer": "Many teams run plain on-demand Auto Scaling groups, often single-instance ones, on either a legacy launch configuration or a newer launch template. To adopt Spot the native way you have to redesign each of those groups: convert it to a launch template with a mixed instances policy, choose the Spot instance types and allocation strategy, and set the Spot/on-demand split. AutoSpotting instead works with your existing groups as they are, launch configuration or launch template, by simply adding a tag, without changing the group configuration in any way.\n\nKey differences:\n\n- **Automatic on-demand fail-over.** If Spot capacity runs out across all compatible types, AutoSpotting falls back to on-demand instances and returns to Spot once the market recovers. The native integration can leave a group short of capacity when all its configured Spot types are unavailable.\n- **Fleet-wide rollout with no config changes.** Enable it across a whole account or AWS Organization in opt-out mode without converting a single group to launch templates.\n- **Automated instance-type selection.** It picks compatible types based on your existing on-demand instance, so you don't maintain per-group instance-type lists.\n- **Works out of the box** with Spot termination notices, load-balancer draining, and Elastic Beanstalk environments.\n- **No lock-in.** Turn it on or off at will (including from CI/CD or on a schedule) and revert to pure on-demand instantly.\n\nIn fairness, the trade-offs are slightly higher instance churn, a small runtime and maintenance cost, and a usage-based fee if you use the managed binaries rather than building from source."
    },
    {
      "question": "How much can I save with AutoSpotting?",
      "answer": "Typical savings are in the 60-90% range for spot-compatible workloads, usually seen when using spot instances. Actual savings depend on your region, instance types used, and spot market conditions."
    },
    {
      "question": "Is AutoSpotting production-ready?",
      "answer": "Yes! AutoSpotting has been battle-tested since 2016 by thousands of companies including major enterprises like Samsung, Expedia, and Mozilla. It includes diversified failover with automatic revert to on-demand instances when spot capacity becomes unavailable."
    },
    {
      "question": "Do I need to change my infrastructure?",
      "answer": "No! AutoSpotting works with your existing AutoScaling groups, launch configurations, and launch templates without any changes. Just add the 'spot-enabled=true' tag to your AutoScaling groups."
    },
    {
      "question": "Where does AutoSpotting run?",
      "answer": "AutoSpotting runs entirely within your AWS account as Lambda functions. There's no SaaS backend and it doesn't send any data externally. You maintain complete control and visibility."
    },
    {
      "question": "How is the commercial version priced?",
      "answer": "Usage-based pricing: up to 10% of the savings generated, billed through AWS Marketplace. For every $1000 in monthly spot savings, you pay approximately $100. Includes a perpetual free tier for small instances (T3/T4g nano or micro). Enterprise tier with flat pricing based on footprint is also available."
    },
    {
      "question": "What happens if spot instances aren't available?",
      "answer": "AutoSpotting automatically falls back to on-demand instances when spot capacity becomes unavailable. You can also configure it to maintain a minimum number of on-demand instances in each AutoScaling group."
    },
    {
      "question": "Does it work with ECS, EKS, or Elastic Beanstalk?",
      "answer": "Yes! AutoSpotting works with any service backed by AutoScaling groups, including managed services like ECS, EKS, and Elastic Beanstalk. No special configuration needed."
    },
    {
      "question": "Can I use it with load balancers?",
      "answer": "Absolutely. AutoSpotting properly handles traffic draining for instances behind Elastic Load Balancers (ELB, ALB, NLB) before terminating them."
    },
    {
      "question": "How do I install AutoSpotting?",
      "answer": "Installation takes just minutes using CloudFormation or Terraform. The commercial version is available directly from AWS Marketplace for even simpler deployment. For the community edition, build from source on GitHub."
    },
    {
      "question": "What if I want to stop using AutoSpotting?",
      "answer": "No vendor lock-in! Simply uninstall AutoSpotting and your AutoScaling groups will eventually revert to fully on-demand instances. For AWS Marketplace subscriptions, you can cancel anytime."
    }
  ],
  "contact": {
    "title": "Still have questions?",
    "description": "If you need help or have any further questions about AutoSpotting, reach out and we'll do our best to help you.",
    "buttons": [
      {
        "text": "Email Us",
        "url": "mailto:contact@autospotting.io?subject=Question%20about%20AutoSpotting&body=Hi%20AutoSpotting%20team%2C%0D%0A%0D%0AI%20have%20a%20question%20about%20AutoSpotting%3A%0D%0A%0D%0A%0D%0AA%20bit%20about%20my%20setup%20%28optional%2C%20helps%20us%20answer%20faster%29%3A%0D%0A-%20AWS%20region%28s%29%3A%0D%0A-%20Approx.%20number%20of%20instances%20or%20monthly%20EC2%20spend%3A%0D%0A-%20Your%20use%20case%20and%20tech%20stack%20%28e.g.%20API%2C%20backend%2C%20frontend%2C%20batch%20jobs%29%3A%0D%0A%0D%0AThanks%21"
      },
      {
        "text": "Book a Call",
        "url": "https://calendly.com/cristi-leanercloud/30min"
      }
    ]
  }
}
{{< /faq >}}

<div id="contact"></div>

{{< section-container >}}

{{< cta
    title="Start Saving on AWS Costs Now"
    description="Join thousands of companies already using AutoSpotting to optimize their AWS infrastructure"
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}

{{< /section-container >}}
