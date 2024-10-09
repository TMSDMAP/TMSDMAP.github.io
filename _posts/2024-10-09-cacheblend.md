---
layout: post
title: "How LMCache Speeds Up RAG by 4.5x By One Line of Change"
thumbnail-img: /assets/img/cacheblend-snapshot.png
share-img: /assets/img/cacheblend-snapshot.png
author: LMCache Team
image: /assets/img/cacheblend-snapshot.png
---
<br>


<span style="font-size:1.3em;">**TL;DR:** Your RAG can run up to **<span style="color:#2AA198;">4.5×</span>** faster by pairing vLLM with LMCache </span>.

<br>

**[[💻 Source code]](https://github.com/LMCache/LMCache) &nbsp;  [[📚 Paper]](https://arxiv.org/abs/2405.16444) will appear in the 10th ACM EuroSys (European Conference on Computer Systems) 2025 &nbsp; [[🎬 3-minute introduction video]](https://m.youtube.com/watch?v=rINy7mFyRAU)**

## The Problem: RAG is WAY TOO SLOW

Retrieval-Augmented Generation (RAG) has become a key technique in improving the performance of large language models (LLMs). 
RAG supplements user queries with relevant text chunks retrieved from external databases, enabling models to generate highly accurate and context-aware responses. 
Typically, adding more texts as context of the input boosts the quality of LLM generation 
[[Report1]](https://www.mongodb.com/developer/products/atlas/choosing-chunking-strategy-rag/)
[[Report2]](https://adasci.org/chunking-strategies-for-rag-in-generative-ai/)
[[Report3]](https://www.datacamp.com/tutorial/how-to-improve-rag-performance-5-key-techniques-with-examples).

But while RAG enhances the quality of LLM outputs, it isn’t perfect---**speed** is a problem.


<div align="center">
<img src="/assets/img/cacheblend-fig1.png" alt="Icon" style="width:600px; vertical-align:middle;">
</div>


The biggest hurdle for RAG systems lies in the delay of processing the  multiple text chunks in the LLM’s input. 
This is because the model needs to compute the KV caches for each token in the input before the first token is generated.

What’s worse, in RAG, *the traditional prefix-based KV caching can be almost as slow as doing no caching at all*.
This is because the multiple input text chunks are selected dynamically, so when they are concatenated in the input, all of them (except the first one) will NOT be the prefix.


## How LMCache Speeds Up RAG

Here’s where LMCache's *CacheBlend* technique changes the game.

<div align="center">
<img src="/assets/img/cacheblend-fig2.png" alt="Icon" style="width:700px; vertical-align:middle;">
</div>

CacheBlend can reuse KV caches from **all** chunks—whether they are the prefix or not, while maintaining generation quality. 
CacheBlend achieves this by selectively recomputing the KV cache of a *small* set of *critical tokens*. 
In the meantime, the small extra delay for recomputing some tokens can be pipelined with the retrieval of KV caches. 
In this way, CacheBlend significantly speeds up the TTFT by up to **4.5X** (~15% recomputation on non-prefix KV caches). 

## Beyond RAG: CacheBlend and Prompt Templates in Agent-Based Applications

While CacheBlend speeds up RAG, its benefits extend even further—particularly into agent-based applications.

In these applications, agents often follow predefined prompt templates to handle dynamic interactions.CacheBlend minimizes the computation required for template-based systems through fast reuse of the stored KV caches of the prompt templates. 
This is especially useful in customer service bots, recommendation engines, and automated assistants, where agents need to pull from a consistent set of prompts and context to perform well.

## Try it yourself!


First clone the demo:
```bash
git clone https://github.com/LMCache/demo
pip install openai streamlit
cd demo/demo3-KV-blending
```

Pull and configure the docker with:
```bash
cp run-server.sh.template run-server.sh
vim run-server.sh
```

Edit the following lines based on your local environment:
```bash
MODEL=mistralai/Mistral-7B-Instruct-v0.2	# LLM model name
LOCAL_HF_HOME=                          	# the HF_HOME on local machine. vLLM will try finding/dowloading the models here
HF_TOKEN=                               	# (optional) the huggingface token to access some special models
```

Then, start the docker images:
```bash
bash ./run-server.sh
```

Start the frontend
```bash
streamlit run frontend.py
```

Now you are good to go!
In the streamlit frontend, you can tick the `EnableLMCacheBlend optimization` checkbox to enable faster RAG.
