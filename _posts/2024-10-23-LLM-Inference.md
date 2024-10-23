---
layout: post
title:  "LLM inference with llama.cpp framework"
---


# [Motivation](https://github.com/avble/av_llm)
The Large language models (LLMs) has evolutionized the NPL and it has pushed text generation applications, such as chat, assistant to the next level by producing text that displays a high level of understanding and fluency.

<br>
This post intents to demonstrate a [tool/application](https://github.com/avble/av_llm) that is collection of the software libraries (components) for making an simple LLM inference tool/application
It is more engineering perspective that is to create the real application utilizing the LLM. In stead of looking into NLP concepts such as Neural network.
<br>

The [av_llm tool](https://github.com/avble/av_llm) can be used to explore/experiment the LLM inference more and figure out how it is applied in each domain.

You can go to `quick started` section of [av_llm tool](https://github.com/avble/av_llm) to get started with.

[This tools](https://github.com/avble/av_llm) consists a Web UI (javascript), and OpenAI API server (C++) and LLM inferences (llama.cpp)


## Features
* OpenAI API compatible server (chat and completion endpoint)
* Simple Web UI for explore/debug
* Currently, it only supports chat application

## Tech-stack
* OpenAI API compatible server: [av_connect http server](https://github.com/avble/av_connect.git)
* LLM Inference: [llama.cp](https://github.com/ggerganov/llama.cpp.git)
* Web UI: Provide a simple web UI interface to explore/experiment


## Some snapshot
![demo-2](https://github.com/avble/av_llm/blob/main/image/demo_3.JPG?raw=true)
<br>
<br>
![demo-1](https://github.com/avble/av_llm/blob/main/image/demo_1.JPG?raw=true)


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
