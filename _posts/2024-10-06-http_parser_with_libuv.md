---
layout: post
title:  "[libuv] Demonstration of http server by using http-parser and libuv"
---

I have created a [http server](https://github.com/avble/http_parser-libuv) using [http-parser](https://github.com/nodejs/http-parser) and [libuv](https://github.com/libuv/libuv) which are well-know for parsing http request and I/O event library, respectively.

libuv is well-know as an IO event library used by nodejs. http-parser is well-known for its using as http parser in nodejs

The performance is quite impressive on `11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz` and `WSL-Ubuntu 22.04` 
167,000 request per seconde

ab (Apache Benchmark) command for performance test
``` shell
$ ab -k -c 50 -n 100000 127.0.0.1:12345/route_01
```
