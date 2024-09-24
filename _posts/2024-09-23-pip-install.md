---
layout: post
title: "vLLM user? Change just ONE line of code to unlock 100x more KV cache storage power!"
thumbnail-img: /assets/img/lmcache-pip.png
share-img: /assets/img/lmcache-pip.png
author: LMCache Team
image: /assets/img/lmcache-pip.png
---
<br>

<div align="center">
<img src="/assets/img/lmcache-pip.png" alt="Icon" style="width: 80%; vertical-align:middle;">
</div>

<!-- ------------ -->

<!-- <span style="font-size:1.3em; color=red;">**TL;DR:** <img src="/assets/img/lmcache-logo-small.png" alt="Icon" style="width:150px; vertical-align:middle;"> *turboboosts vLLM with <span style="font-size:1.3em; color=red;">**7×**</span> faster access to <span style="font-size:1.3em; color=red;">**100×**</span> more KV caches.*</span> -->


Are you a vLLM user? Get 100x more KV cache storage space for your multi-round conversation and document QA using LMCache! 


## Offline inference

For offline inference, you can use LMCache within two steps:

First run

```bash
pip install lmcache lmcache_vllm
```

And then change 

```python
import vllm
``` 

to

```python
from lmcache_vllm import vllm
```

and now you are good to go! 

Like in the following example

```python
"""
simply change
    import vllm
to
"""
from lmcache_vllm import vllm
"""
and you are good to go!
"""

prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]
sampling_params = vllm.SamplingParams(temperature=0.8, top_p=0.95)

llm = vllm.LLM(model="facebook/opt-125m")

outputs = llm.generate(prompts, sampling_params)

# Print the outputs.
for output in outputs:
    prompt = output.prompt
    generated_text = output.outputs[0].text
    print(f"Prompt: {prompt!r}, Generated text: {generated_text!r}")
```


## Online serving

If you prefer using vLLM through its OpenAI API server, you can also use LMCache within 2 steps:

First run
```bash
pip install lmcache lmcache_vllm
```
and then replace `vllm serve` to `lmcache_vllm serve`.
For example, you can change
```bash
vllm serve lmsys/longchat-7b-16k --gpu-memory-utilization 0.8
```
to
```bash
lmcache_vllm serve lmsys/longchat-7b-16k --gpu-memory-utilization 0.8
```
and now your lmcache-augmented vLLM server is up and ready for use!

<br>

## Contact Us

Interested? Check our github repo in [github.com/LMCache/LMCache](https://github.com/LMCache/LMCache)!

