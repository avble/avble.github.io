---
layout: post
title:  "LLM inference - function call"
---

# Introduction

Function calling is a feature that the given a `description` in natural language, the LLM inference can produce a function description in maybe json format. So that the application can call appropriate function in program. 
In order to achieve it, it is neccessary to let LLM model aware of all function description
This post presents steps for function calling.

First, pre-trained model is given the below `function description`
``` json
[
    {
        "type": "function",
        "function": {"name":"get_all_devices","description":"get all lights","parameters":{}}
    },
    {
        "type": "function",
        "function": {
            "name": "get_on_off_status",
            "description": "Get onoff status of a light",
            "parameters": {"type":"object","properties":{"location":{"type":"string","description":"name of light"}},"required":["location"]}
        }
    }
     
]
```

Then if the text `all devices` is given, the LLM inference is able to produces the following json response

```json
[{"name":"get_all_devices","arguments":{}}]
```

if the text `the status of kitchen light`, the LLM inference is able produce the below json
```json
 [{"name":"get_on_off_status","arguments":{"location":"kitchen"}}]
```

Once the json is received by application, it is parsed and call appropriate function and then forward result to user.

The below is a snapshot of application during experimental.

![screenshot](/assets/img/llm_inference_01.png)



# Notes
* The first impressive to me is that, different words `all devices`, `all lights`, `display all lights for me`, or even a spell mistake `all devic to me`, are given, the LLM inference can provide a unique response `[{"name":"get_all_devices","arguments":{}}]`. In addition, the LLM inference can produce the correct json syntax.
* At the time of this experiment, LLM inference does not stop producing token once the response json is completed. So, the application should handle this case by checking if the json is completed and does not request LLM infer the next token. 



# Reference
* https://platform.openai.com/docs/guides/function-calling
* https://docs.llama-api.com/essentials/function
* https://github.com/ggerganov/llama.cpp
  
