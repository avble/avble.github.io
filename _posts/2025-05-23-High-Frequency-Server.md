---
layout: post
title: "C/C++ server for High-Frequency application"
---

C/C++ is well-know for its performance. And IO event is emerging technique used in various well-known http framework these days.
To name a few, nodejs (javascript), aiohttp (python), boost-beast (C++), boost-asio (C++).

For application which required high-frequency, selecting a right framework/library is very important.

This work is to develop 2 http servers in C++ and then their performance are compared with other frameworks/libraries.
These two developed servers are [http-parser-with-libuv](https://github.com/avble/http_parser-libuv) and [http-parser-with-libevent](https://github.com/avble/http-parser-with-libevent)

The experimental result shows that these two severs are well suited for application which the high-frequency is required.

The below table shows its performance in term of `request per second`.

ab (Apache Benchmark) command for performance test

```shell
$ ab -k -c 50 -n 100000 127.0.0.1:12345/route_01
```

| http server                                                                     | Request per second | Remark                                                                                                               |
| ------------------------------------------------------------------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------- |
| [http-parser-with-libuv](https://github.com/avble/http_parser-libuv)            | ~167,000           | using llhttp-parser + libuv                                                                                          |
| [http-parser-with-libevent](https://github.com/avble/http-parser-with-libevent) | ~150,000           | using llhttp-parser (not internal http of libevent)                                                                  |
| internal http libevent                                                          | ~95,000            | [bench_http](https://github.com/libevent/libevent/blob/master/test/bench_http.c) of libevent (release-2.1.12-stable) |
| nodejs                                                                          | 12,000             | v12.22.9                                                                                                             |
| asiohttp                                                                        | 11,000             | 3.10.6                                                                                                               |
| flask                                                                           | 697                | 3.0.3                                                                                                                |

Note: performance test environment

- Hardware: `11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz`
- OS: `microsoft-standard-WSL2` (Ubuntu 22.04.2 LTS)
