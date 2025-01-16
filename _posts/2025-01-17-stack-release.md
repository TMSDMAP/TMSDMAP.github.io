---
layout: post
title: "Enabling High Performance and Easy Deployment of vLLM with “LMStack”"
thumbnail-img: /assets/img/stack-thumbnail.png
share-img: /assets/img/stack-thumbnail.png
author: LMCache Team
image: /assets/img/stack-thumbnail.png
---
<br>


## TL;DR
- **vLLM** boasts the largest open-source community, but what does it take to transform vLLM from the best single-node LLM engine to a premier LLM serving system?
- **Today, we release “LMStack”**, a vLLM-based full inference stack that introduces two major advantages:
  - **10x better performance** (3-10x lower response delay & 2-5x higher throughput) with prefix-aware request routing and KV-cache sharing.
  - **Easy cluster deployment** with built-in support for fault tolerance, autoscaling, and observability.
- And the best part? It’s **open-source**—so everyone can get started right away! [**Repo link in vLLM-project**]


# The Context
<!-- Over the past year, LLM inference has raced to the forefront, powering everything from chatbots to code assistants and beyond. It’s quickly becoming critical infrastructure, much like the cloud was to big data, cellular was to mobile apps, and CDNs were (and still are!) to the broader Internet. -->

*In the AI arms race, it’s no longer just about who has the best model—it’s about **who has the best LLM inference system**.*

**vLLM** has taken the open-source community by storm, with unparalleled hardware and model support plus an active ecosystem of top-notch contributors. But until now, vLLM has mostly focused on **single-node** deployments.

How do we extend its power into a **full-stack** inference system that any organization can deploy at scale with *high reliability*, *high throughput*, and *low latency*? That’s precisely why we built **LMStack**.

<div align="center">
<img src="/assets/img/stack-thumbnail.png" alt="Icon" style="width: 60%; vertical-align:middle;">
</div>

# Introduce "*LMStack*"
**LMStack** is an open-source **reference implementation** of a **inference stack** built on top of vLLM, designed to run seamlessly on a cluster of GPU nodes. It adds four critical functionalities that complement vLLM’s native strengths:
- **KV cache sharing & storage** to speed up inference when context is reused (powered by the popular [**LMCache**](https://github.com/LMCache/LMCache) project).
- **Prefix-aware routing** that sends queries to the vLLM instance already holding the relevant context KV cache.
- **Observability** of individual engine status and query-level metrics (TTFT, TBT, throughput).
- **Autoscaling** to handle dynamics of workloads.

### Comparison with Alternatives:

Below is a quick snapshot comparing LMStack with its closest counterparts:
<div align="center">
<img src="/assets/img/stack-table.png" alt="Icon" style="width: 70%; vertical-align:middle;">
</div>

### The Design
The LMStack architecture builds on top of vLLM’s powerful single-node engine to provide a cluster-wide solution. 

At a high level:
- Clients send LLM inference requests.
- Prefix-aware routing checks if the requested context is already cached somewhere, then forwards the request to the node with the relevant cache.
- Autoscaling and a cluster manager watch the overall load and spin up new vLLM nodes if needed.
- Observability modules gather metrics like TTFT (Time-To-First-Token), TBT (Time-Between-Tokens), and throughput, giving you real-time insights into your system’s health.

<div align="center">
<img src="/assets/img/stack-overview.png" alt="Icon" style="width: 90%; vertical-align:middle;">
</div>

# Advantage #1: Easy Deployment

Requirement: 
- kubernetes environment (local or cloud)
- GPU
- (Optional) PersistentVolume which contains the model weights
  - If not present, vLLM pods will download the model weights themselves
```bash
helm upgrade --install vllm-stack <Helm chart url> 
# <Helm chart url> can be something like https://vllm-project.github.io/stack-helm-charts
```

# Advantage #2: Better Performance
Our preliminary tests show LMStack outperforms other setups across key metrics. 

<div align="center">
<img src="/assets/img/stack-perf.png" alt="Icon" style="width: 60%; vertical-align:middle;">
</div>

# Advantage #3: Effortless Monitoring
Observability panel

<div align="center">
<img src="/assets/img/stack-panel.png" alt="Icon" style="width: 70%; vertical-align:middle;">
</div>


## Conclusion
We’re thrilled to unveil **LMStack**—the next step in transforming vLLM from a best-in-class single-node engine into a full-scale LLM serving system. 
We believe LMStack will open new doors for organizations seeking to build, test, and deploy LLM applications at scale without sacrificing performance or simplicity.

If you’re as excited as we are, don’t wait!
- **Clone the repo: [Link]**
- **Kick the tires**
- **Let us know what you think!**
Join us, and let’s build a future where every application can harness the power of LLM inference—reliably, at scale, and without breaking a sweat.

*Happy deploying!*
