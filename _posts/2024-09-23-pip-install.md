---
layout: post
title: "vLLM user? Get 100x KV cache storage by only chaninging one line of code"
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

Are you a vLLM user? Get 100x more KV cache storage space for your multi-round conversation and document QA using LMCache! Just two steps:

- Step 1: `pip install lmcache, lmcache-vllm`
- Step 2: change `import vllm` to `from lmcache_vllm import vllm` and you are good to go! 

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

If you use vLLM's OpenAI API server, you can also use LMCache within 2 steps:
- Step 1: `pip install lmcache, lmcache-vllm`
- Step 2: Replace `vllm serve` by `lmcache_vllm serve` and keep all other command line arguments the same.

<br>
<be>

## Contact Us

Interested? Check our github repo in [github.com/LMCache/LMCache](https://github.com/LMCache/LMCache)!

