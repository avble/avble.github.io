---
layout: post
title:  "[High-traffic http server] by http-parser with libevent"
---

I have created a [minimun http](https://github.com/avble/http-parser-with-libevent) using [http-parser](https://github.com/nodejs/llhttp) and [libevent](https://github.com/libevent/libevent) which are well-know for parsing http request and I/O event library, respectively.

The below table shows its performance in term of `request per second`. It is very impressive.
It surpasses several well-know/modern web framework.

ab command for performance test
``` shell
$ ab -k -c 50 -n 100000 127.0.0.1:12345/route_01
```

| http server | Request per second | Remark |
|----|----|----|
| [http-parser-with-libevent](https://github.com/avble/http-parser-with-libevent)  |      ~150,000      |  using llhttp-parser (not internal http of libevent) |
| internal http libevent  |      ~95,000      |  [bench_http](https://github.com/libevent/libevent/blob/master/test/bench_http.c) of libevent (release-2.1.12-stable) |
| nodejs   |    12,000  | v12.22.9 |
| asiohttp | 11,000 | 3.10.6 |
| flask   | 697 | 3.0.3 |


Note: performance test environment
* Hardware: `11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz` 
* OS: `microsoft-standard-WSL2` (Ubuntu 22.04.2 LTS)
