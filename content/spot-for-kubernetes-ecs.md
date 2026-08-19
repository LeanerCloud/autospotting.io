---
title: "Spot for Kubernetes and ECS"
seo_title: "Running Spot Instances on Kubernetes (EKS) and ECS in AWS"
description: "Run EC2 Spot on EKS, Kubernetes, and ECS with AutoSpotting. Works with any AutoScaling-backed service, with on-demand failover and no launch-template rewrite."
layout: "landing"
url: "/spot-for-kubernetes-ecs/"
---

{{< hero
    headline="Running Spot on Kubernetes (EKS) and ECS"
    sub_headline="Container schedulers are built to move work off a node that goes away, which makes them a natural fit for Spot at 60-90% off.<br><br>AutoSpotting brings Spot to EKS, self-managed Kubernetes, and ECS through the AutoScaling groups you already run, with automatic on-demand fallback and no launch-template rewrite."
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
    <h2>Why containers and Spot fit together</h2>
    <p>ECS and Kubernetes already assume nodes come and go. When a node disappears, the scheduler reschedules its tasks or pods onto remaining capacity. That is the exact behavior you need to run on Spot, where AWS can reclaim a node on a two-minute notice. If your services carry no per-node state and sit behind a load balancer or a service mesh, moving the underlying compute to Spot changes the bill by 60-90% without changing how the workload behaves.</p>
    <p>The savings are concrete. One team evaluating AutoSpotting for their EKS setup measured about 60% savings on the test EC2 instance in the demo alone. Another user ran it on a personal project years earlier and had it saving roughly $900 per month before proposing it at work.</p>

    <h2>It works through your AutoScaling groups</h2>
    <p>EKS managed node groups, self-managed Kubernetes node pools, and ECS container instances all run on EC2 AutoScaling groups underneath. AutoSpotting works with any service backed by an AutoScaling group, so it plugs into that layer directly. You add a tag to the group. You do not convert it to a launch template with a mixed instances policy, and you do not rewrite launch templates or maintain per-group instance-type lists.</p>
    <p>From there AutoSpotting picks compatible Spot instance types based on your existing on-demand type, diversifies across them to spread interruption risk, and falls back to on-demand when Spot capacity runs short across all compatible pools. Everything runs inside your own AWS account with no SaaS backend and no cross-account access. Because it is driven by a tag, you can enable it on one node group, confirm the scheduler behaves as expected, and roll it out across the rest, or remove the tag to revert. One AutoSpotting user runs about 30% of their production fleet on Spot this way, without configuration changes and with on-demand failover underneath.</p>

    <h2>ECS draining, handled correctly</h2>
    <p>Containers on Spot need the node to leave its load balancer cleanly when it is reclaimed, and ECS does not always do this fast enough on its own. An AutoSpotting user running ECS reported dropped connections and 5xx errors during Spot terminations, because ECS was slow to start draining, sometimes beginning load-balancer deregistration only seconds before the instance shut down, and sometimes after it was already gone.</p>
    <p>AutoSpotting addresses this by deregistering the instance from its target group within about 10 seconds of the Spot termination event, and by extending that instance and task draining to ECS. Connections drain during the two-minute window instead of being cut. If you run ECS with Spot, this is the behavior that keeps interruptions invisible to your users.</p>

    <h2>Where it fits with Karpenter and Cluster Autoscaler</h2>
    <p>Kubernetes has its own scaling tools. Cluster Autoscaler and Karpenter provision nodes in response to pending pods, and Karpenter can request Spot capacity directly. AutoSpotting operates at the AutoScaling group layer, so it is the right fit when your nodes are backed by managed node groups or self-managed groups and you want Spot with on-demand fallback without adopting a different provisioner or re-architecting your node setup. For ECS, Elastic Beanstalk, and any other AutoScaling-group-backed service, it is the same tag on the same kind of group.</p>
    <p>One caveat worth knowing: EKS managed node groups running Spot, and the native Auto Scaling group Spot integration in general, do not fall back to on-demand when Spot is exhausted across all configured instance types. They launch fewer nodes instead, so the cluster silently drops to reduced capacity until Spot frees up, often during high-demand periods such as the end-of-year holiday season. AutoSpotting fails over to on-demand automatically, so the group keeps full capacity.</p>
  </div>
</section>

{{< faq >}}
{
  "title": "Spot for Kubernetes and ECS FAQ",
  "questions": [
    {
      "question": "Does AutoSpotting work with EKS and ECS?",
      "answer": "Yes. AutoSpotting works with any service backed by an EC2 AutoScaling group, including EKS managed node groups, self-managed Kubernetes node pools, ECS container instances, and Elastic Beanstalk. You add a tag to the group, with no launch-template rewrite."
    },
    {
      "question": "Do I have to change my launch templates or node groups?",
      "answer": "No. AutoSpotting works with your existing AutoScaling groups as they are, whether they use a launch configuration or a launch template. You do not convert them to a mixed instances policy or maintain per-group instance-type lists. Add the tag and remove it to revert."
    },
    {
      "question": "How does it handle ECS draining when a Spot node is reclaimed?",
      "answer": "It deregisters the instance from its load balancer target group within about 10 seconds of the Spot termination event and extends the same instance and task draining to ECS, so connections drain during the two-minute window rather than being dropped."
    },
    {
      "question": "How does AutoSpotting compare to Karpenter for Spot?",
      "answer": "Karpenter provisions nodes directly and can request Spot itself. AutoSpotting operates at the AutoScaling group layer, so it fits when your nodes run on managed or self-managed node groups and you want Spot with automatic on-demand fallback without adopting a different provisioner. The same approach covers ECS and Elastic Beanstalk."
    }
  ]
}
{{< /faq >}}

<section class="bg-gray-50">
  <div class="py-8 px-4 mx-auto max-w-screen-md lg:py-12 lg:px-6">
    <h2 class="mb-4 text-2xl font-bold text-gray-900">Related</h2>
    <ul class="space-y-2 text-primary-700">
      <li><a class="hover:underline" href="/spot-vs-on-demand/">Spot vs on-demand: when each one makes sense</a></li>
      <li><a class="hover:underline" href="/spot-interruption-handling/">How EC2 Spot interruptions work, and how to handle them</a></li>
    </ul>
  </div>
</section>

{{< cta
    title="Bring Spot to your clusters"
    description="Tag the AutoScaling groups behind your EKS, Kubernetes, or ECS nodes and let AutoSpotting handle diversification, draining, and on-demand fallback."
    primary_button_text="Install from AWS Marketplace"
    primary_button_url="https://aws.amazon.com/marketplace/pp/prodview-6uj4pruhgmun6"
    secondary_button_text="Try Community Edition"
    secondary_button_url="https://github.com/AutoSpotting/AutoSpotting"
    gradient_from="#2563eb"
    gradient_to="#7c3aed"
    gradient_angle="135"
>}}
