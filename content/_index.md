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
    sub_headline="AutoSpotting automatically replaces on-demand instances in AutoScaling groups with Spot clones.<br><br>• Install in minutes<br>• Configure with tags<br>• No launch template changes required"
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="See Pricing"
    secondary_button_url="/#pricing"
    hero_image="/images/savings-before-after.png"
    gradient-from="#1e40af"
    gradient-to="#7c3aed"
    gradient-angle="135"
>}}

<div id="features"></div>

{{< features-section
    title="Making Spot instances safe for production"
    description="AutoSpotting has helped thousands of companies and institutions save over $100,000,000 since 2016"
>}}

{{< feature
    title="Production-Ready Reliability"
    description="Automatic failover to on-demand instances ensures your applications stay running even when spot capacity is unavailable."
    image="/images/features/production.svg"
    features="Zero downtime during spot interruptions,Traffic draining for load balancers,Configurable on-demand instance retention,Trusted by major enterprises worldwide"
    imagePosition="right"
>}}

{{< feature
    title="Install in Minutes"
    description="Get started immediately with CloudFormation or Terraform. Tag your existing AutoScaling groups with 'spot-enabled=true' or use our GUI to estimate savings and enable with a single click - no launch template changes required."
    image="/images/features/friction.svg"
    features="5-minute installation,Works with existing infrastructure,Deploy across multiple AWS accounts with StackSets,Start saving costs today"
    imagePosition="left"
>}}

{{< feature
    title="Your Data Stays Private"
    description="Runs entirely within your AWS account as Lambda functions. No SaaS backend, no external data transmission, minimal IAM permissions required."
    image="/images/features/security.svg"
    features="Complete data privacy,Full auditability and control,Industry-standard security practices,No vendor access to your infrastructure"
    imagePosition="right"
>}}

{{< feature
    title="Works With Your Stack"
    description="Seamless integration with ECS, EKS, Elastic Beanstalk, and any service backed by AutoScaling groups. Fits naturally into your CI/CD pipelines."
    image="/images/features/compatible.svg"
    features="Zero vendor lock-in,Easy to suspend or remove anytime,Complements existing AWS services,Multi-account support"
    imagePosition="left"
>}}

{{< feature
    title="Visual Dashboard & Analytics"
    description="Intuitive web interface to monitor your savings, manage AutoScaling groups, and analyze cost optimization across your AWS infrastructure in real-time."
    image="/images/savings-estimator.png"
    features="Real-time savings tracking,One-click spot enablement for ASGs,Historical cost analysis and reports,Visual savings estimates before enabling"
    imagePosition="right"
>}}

{{< feature
    title="Minimal Cost to Save Big"
    description="Pay only 10% of the savings generated through your AWS bill. Serverless architecture means negligible runtime costs."
    image="/images/features/cost.svg"
    features="No upfront investment,Pay only for results,Free tier for small installations,Cancel anytime with no penalties"
    imagePosition="left"
>}}

{{< feature
    title="Backed By Expert Support"
    description="Stable, tested binaries delivered through AWS Marketplace. Get setup assistance and long-term support from the team that built AutoSpotting."
    image="/images/features/support.svg"
    features="Priority support for paying customers,Help with installation and optimization,Regular updates and improvements,Dedicated team with deep AWS expertise"
    imagePosition="right"
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
        <a href="https://bit.ly/LCSavingsCalculator" target="_blank" rel="noopener" class="inline-block px-6 py-3 text-white bg-primary-600 hover:bg-primary-700 font-medium rounded-lg transition-colors">Savings Calculator</a>
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

{{< client-logos animate="true" >}}

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
        "Must share any software changes with the community"
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
        "Installs in minutes with GUI installer",
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
        "url": "mailto:support@autospotting.io?subject=AutoSpotting%20Question"
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
