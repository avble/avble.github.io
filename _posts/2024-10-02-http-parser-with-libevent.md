---
layout: post
title:  "[High-traffic http server] by http-parser with libevent"
---

[http-parser](https://github.com/nodejs/llhttp) and [libevent](https://github.com/libevent/libevent) are well-know for parsing http request and I/O event library, respectively.

I have created a minimun http erver by using these two library at [http-parser-libevent](https://github.com/avble/http-parser-with-libevent)

The below table shows its performance. It is very impressive.
It surpasses several well-know/modern web framework.

| http server | Request per second | Remark |
|----|----|----|
| (http-parser-with-libevent)[https://github.com/avble/http-parser-with-libevent]  |      ~150,000      |  using llhttp-parser (not internal http of libevent) |
| internal http libevent  |      ~95,000      |  [bench_http](https://github.com/libevent/libevent/blob/master/test/bench_http.c) of libevent (release-2.1.12-stable) |
| nodejs   |    12,000  | v12.22.9 |
| asiohttp | 11,000 | 3.10.6 |
| flask   | 697 | 3.0.3 |
