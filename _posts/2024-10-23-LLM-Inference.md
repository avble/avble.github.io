---
layout: post
title:  "LLM inference - lightweight openAI compatible server with llama.cpp"
---


# [Motivation](https://github.com/avble/av_llm)
The Large language models (LLMs) has evolutionized the NPL and it has pushed text generation applications, such as chat, assistant to the next level by producing text that displays a high level of understanding and fluency.

<br>
This post intents to demonstrate a [tool/application](https://github.com/avble/av_llm) that is collection of the software libraries (components) for making an simple LLM inference tool/application.
It is more engineering perspective that is to explore/apply to the real application utilizing the LLM other than looking into concepts such as Convolutional Neural network.
<br>

The [av_llm tool](https://github.com/avble/av_llm) can be used to explore/experiment the LLM inference more and figure out how it is applied in each domain.

You can go to `quick started` section of [av_llm tool](https://github.com/avble/av_llm) to get started with.

## Tech-stack
* lightweight OpenAI API compatible http server: [av_connect](https://github.com/avble/av_connect.git) http server
* LLM Inference: [llama.cp](https://github.com/ggerganov/llama.cpp.git)
* Web UI: Provide a simple web UI interface to explore/experiment

## supported model
As this simple tool is built on the top of [llama.cp](https://github.com/ggerganov/llama.cpp.git), it should support all models which llama.cpp supports.
Name a few as below

* LLaMA 1
* LLaMA 2
* LLaMA 3
* [Mistral-7B](https://huggingface.co/mistralai/Mistral-7B-v0.1)
* [Mixtral MoE](https://huggingface.co/models?search=mistral-ai/Mixtral)
* [DBRX](https://huggingface.co/databricks/dbrx-instruct)
* [Falcon](https://huggingface.co/models?search=tiiuae/falcon)
* [Chinese-LLaMA-Alpaca](https://github.com/ymcui/Chinese-LLaMA-Alpaca)

## Some snapshot
![demo-2](https://github.com/avble/av_llm/blob/main/image/demo_3.JPG?raw=true)
<br>
<br>
![demo-1](https://github.com/avble/av_llm/blob/main/image/demo_1.JPG?raw=true)


# High-level of LLM inference
As a large language model, llama.cpp works by taking an input text, and then predicting what the next tokens, or words, should be.

In my experimental, I use the sentence `The quick brown fox jumps over the lazy dog` from [wikipedia](https://en.wikipedia.org/wiki/The_quick_brown_fox_jumps_over_the_lazy_dog)

If I give the input `The quick brown fox jumps` to the LLM inference, it will produce the next words as below.
`over the lazy dog.`

An LLM inference (such as llama.cpp) must do several intermediate steps between the given input and the predicted output.
They are `tokenize`, `embeddeding`, `logits`, and `sampling`.
The [post](https://www.omrimallis.com/posts/understanding-how-llm-inference-works-with-llama-cpp/) explanation is reasonable so that we can have a better look at low-level of LLM inference.

# Reference
* https://platform.openai.com/docs/api-reference/introduction
* https://www.omrimallis.com/posts/understanding-how-llm-inference-works-with-llama-cpp/
