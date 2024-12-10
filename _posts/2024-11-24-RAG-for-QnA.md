---
layout: post
title: "RAG - a demonstration of RAG and finetuning pre-trained LLM model for QA LM appliation"
---

# Introduction

Large pre-trained language models have been shown to store factual knowledge in their parameters, and achieve state-of-the-art results when fine-tuned on down-stream NLP tasks, such as question and answer.

Fine-tuning a pre-trained model, it requires the `context` and a list of `question/answer` associated with that `context`.

In order to increase performance of `answer` prediction, the `context` along with `question` is feed to the fine-tuned model. However, in real usage the user does not provide the `context` but only the `question`. This leads to the poor performance (aka hallucinations)

It has been somehow addressed in the paper [2] and RAG method is proposed. In this study, a analogous method to RAG is come up to extract the `context` from query by Similarity search.
Then `context` along with `query` is feed to fine-tunned model to predict answers. 

This study presents a RAG-based method for Q&A down-stream NLP tasks as below figure.
![RAG-based method overview](/assets/img/rag_overview.png)

* The dotted arrow 1, and 2 illustrates creating vector database and fine-tunned model, respectively.
* The solid arrow inllustrates the flow of getting predicted answer based on the given question.

The Jupyter Notebook of this study can be found at [here](https://github.com/avble/llm_things/tree/main/01_rag_qa)

# Data preparation
Data comes from varous sources, the text in image, pdf, or excel files. 
It needs to be prepared so that it can contain as the below major information.

```python
{
    "context": "this is the context 1",
    "qas": [{
        "question": "question 1",
        "answer": "answer 01"
    },
    {
        "question": "question 2",
        "answer": "answer 03"
    },
    ]
}
```

The [dataset](https://huggingface.co/datasets/rajpurkar/squad) is used. and thanks author for this study.

# Vector database
The below is representation of dataset.
`embedding` vector is computed by feed the question to an embedding LLM such as, [TencentBAC/Conan-embedding-v1](https://huggingface.co/TencentBAC/Conan-embedding-v1)

``` python
[
    {
        "embedding": [0.1, 0.2, ... , 0.3]
        "context": "this is the context 1"

    }
    {
        "embedding": [0.1, 0.2, ... , 0.3]
        "context": "this is the context 1"
    }
]
```

# Fine tuning for the model
Thanks [simpletransformers](https://pypi.org/project/simpletransformers/) for making fine-tooling process more simple.
It takes about 2 (two) minutes to do fine-tunning the model on google colab T4 GPU resouce.

# Predict answer for a question
An embedding vector is computed from given question by [TencentBAC/Conan-embedding-v1 LLM model](https://huggingface.co/TencentBAC/Conan-embedding-v1). And a context is obtained by a similarity search of this embedding vector with vector database which is prepared in the preceeding step. 

A tuple (question, context) is feed to fine-tunned model to predict the answer.

# mini QnA app
The gradio is used to make mini LM application. The below is some snapshots of it.

![lm app 1](/assets/img/rag_gradio_app_1.png)

![lm app 2](/assets/img/rag_gradio_app_2.png)



# Future work
* Data preparation is very first step in Q&A LM application. And it is very specific domain. Should any dataset is obatained, will apply to demonstrate this study.
* Similarity search is also a non-trivial step, which is to extract `context`. It significantly contributes to the performance of fine-tuned model. It is highly considered to make use of Faiss at [3] when its requirements are more complex.
* Cause, detection, and mitigation of hallucinations have been summarized at [4]. It is worth of reading to improve the LM QnA performance

# Reference
* [1] https://arxiv.org/pdf/1810.04805, BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding
* [2] https://arxiv.org/pdf/2005.11401, Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks
* [3] https://github.com/facebookresearch/faiss, vector database
* [4] https://arxiv.org/pdf/2311.05232, Hallucination survey

