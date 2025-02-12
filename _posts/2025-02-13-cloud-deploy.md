---
layout: post
title: "Spin up your own LLM inference cluster on cloud services with “vLLM production-stack”"
thumbnail-img: /assets/img/stack-thumbnail.png
share-img: /assets/img/stack-thumbnail.png
author: LMCache Team
image: /assets/img/stack-thumbnail.png
---
<br>


## TL;DR
- vLLM boasts the largest open-source community in LLM serving, and **“vLLM production-stack”** offers a vLLM-based full inference stack with **10x** better performance and Easy cluster **management**.
- Today, we demonstrate how to deploy **“vLLM production-stack”** on three major cloud platforms, AWS, GCP and Lambda Labs.
- Try deploy this **open-source** repository today on the major cloud providers now! 
[[Github Link]](https://github.com/vllm-project/production-stack) | [[AWS EKS Tutorial Video]](xxx) | [[Lambda VM Tutorial Video]]()


# The Context
<!-- Over the past year, LLM inference has raced to the forefront, powering everything from chatbots to code assistants and beyond. It’s quickly becoming critical infrastructure, much like the cloud was to big data, cellular was to mobile apps, and CDNs were (and still are!) to the broader Internet. -->


**vLLM** has taken the open-source community by storm, with unparalleled hardware and model support plus an active ecosystem of top-notch contributors. But until now, vLLM has mostly focused on **single-node** deployments.

**vLLM Production-stack** is an open-source **reference implementation** of an **inference stack** built on top of vLLM, designed to run seamlessly on a cluster of GPU nodes. It adds four critical functionalities that complement vLLM’s native strengths.

Below is a quick snapshot comparing vLLM production-stack with its closest counterparts:
<div align="center">
<img src="/assets/img/stack-table.png" alt="Icon" style="width: 90%; vertical-align:middle;">
</div>

 
 \
vLLM production-stack offers superior performance than other LLM serving solutions by achieving higher throughput through smart routing and KV cache sharing\:
<div align="center">
<img src="/assets/img/stack-ttft.png" alt="Icon" style="width: 60%; vertical-align:middle;">
</div>



# Deploying "production-stack" in cloud
In this section, we will go through the general steps to set up the vLLM production-stack service in the cloud.
## Step 1: Set up Kubernetes and Helm
### On Lambda Labs VM
Otherwise you can go through this [tutorial]() to set up K8S on a single node. If you want to deploy K8S on multiple separate nodes, you can go through this [tutorial]().

### On EKS
If your cloud provider already provides a Kubernetes service (ex. EKS on AWS, GKE on GCP, Azure K8S). You can follow the instruction to start the Kubernetes cluster and add nodes with appropriate GPUs.

## Step 2: Deploy the production-stack
Now you have kubernetes, it is time to deploy production-stack! 

Create a yaml file as shown below. Be sure to include your model name, model url, replica-count, and vLLM configurations in the file.

### Example.yaml
```yaml
servingEngineSpec:
  modelSpec:
  - name: "llama3"
    repository: "vllm/vllm-openai"
    tag: "latest"
    modelURL: "meta-llama/Llama-3.1-8B-Instruct"
    replicaCount: 1

    requestCPU: 10
    requestMemory: "16Gi"
    requestGPU: 1

    pvcStorage: "50Gi"

    vllmConfig:
      enableChunkedPrefill: false
      enablePrefixCaching: false
      maxModelLen: 16384
      dtype: "bfloat16"
      extraArgs: ["--disable-log-requests", "--gpu-memory-utilization", "0.8"]

    hf_token: <YOUR HF TOKEN>
```

Now deploy this configuration using Helm:

```bash
helm repo add vllm https://vllm-project.github.io/production-stack
helm install vllm vllm/vllm-stack -f Example.yaml
```

## Step 3: Send requests and test!  

## Conclusion
We’re thrilled to unveil **vLLM Production Stack**—the next step in transforming vLLM from a best-in-class single-node engine into a full-scale LLM serving system. 
We believe the vLL stack will open new doors for organizations seeking to build, test, and deploy LLM applications at scale without sacrificing performance or simplicity.

If you’re as excited as we are, don’t wait!
- **Clone the repo: [https://github.com/vllm-project/production-stack](https://github.com/vllm-project/production-stack)**
- **Kick the tires**
- **Let us know what you think!**
- **[Interest Form](https://forms.gle/mQfQDUXbKfp2St1z7)**


Join us to build a future where every application can harness the power of LLM inference—reliably, at scale, and without breaking a sweat.
*Happy deploying!*

Contacts:
- **vLLM [slack](https://slack.vllm.ai/)**
- **LMCache [slack](https://join.slack.com/t/lmcacheworkspace/shared_invite/zt-2viziwhue-5Amprc9k5hcIdXT7XevTaQ)**
