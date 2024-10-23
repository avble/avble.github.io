---
layout: post
title:  "LLM inference with llama.cpp framework"
---

# Motivation
I am a software engineer and interested in the MLL inference and its appliation.
<br>
This post intents to demonstrate the software libraries (components) connected together for making an simple LLM inference tool/application
It is more engineering perspective that is to create the real application utilizing the LLM. In stead of looking into NLP concepts such as Neural network.
<br>

It motivates me to create a [av_llm tool](https://github.com/avble/av_llm) so that I can research/explore the real/potential capability of LLM inference and figure out how it is applied in each domain.

You can go to `quick started` section of [av_llm tool](https://github.com/avble/av_llm) to get started with.

This tools consists a Web UI (javascript), and OpenAI API server (C++) and LLM inferences (llama.cpp)


## Features
* OpenAI API compatible server (chat and completion endpoint)
* Simple Web UI for explore/debug
* Currently, it only supports chat application

## Tech-stack
* OpenAI API compatible server: [av_connect http server](https://github.com/avble/av_connect.git)
* LLM Inference: [llama.cp](https://github.com/ggerganov/llama.cpp.git)
* Web UI: Provide a simple web UI interface to explore/experiment


## Some snapshot
![demo-1](https://github.com/avble/av_llm/blob/main/image/demo_1.JPG?raw=true)
![demo-2](https://github.com/avble/av_llm/blob/main/image/demo_2.JPG?raw=true)


# High-level of LLM inference
As a large language model, llama.cpp works by taking an input text, and then predicting what the next tokens, or words, should be.

In my experimental, I use the sentenc `The quick brown fox jumps over the lazy dog` from [wikipedia](https://en.wikipedia.org/wiki/The_quick_brown_fox_jumps_over_the_lazy_dog)

If I give the input `The quick brown fox jumps` to the LLM inference, it will produce the next words as below.
`over the lazy dog.`

An LLM inference (such as llama.cpp) must do several intermediate steps between the given input and the predicted output.
They are tokenize, embeddeding, logits, and sampling.
The [post](https://www.omrimallis.com/posts/understanding-how-llm-inference-works-with-llama-cpp/) explanation is reasonable so that we can have a better look at low-level of LLM inference.

# Reference
* https://platform.openai.com/docs/api-reference/introduction
* https://www.omrimallis.com/posts/understanding-how-llm-inference-works-with-llama-cpp/
