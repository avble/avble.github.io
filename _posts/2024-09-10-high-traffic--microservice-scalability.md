---
layout: post
title:  "A general networking libraries for making network (client/server) application"
---

# Introduction
IO event is emerging technique used in various well-known http framework these days.
To name a few, nodejs (javascript), aiohttp (python), boost-beast (C++), boost-asio (C++)

I have started [av_connect](https://github.com/avble/av_connect) which is collection of networking stuff for making network (client/server) application.

av_connect presents an intutive way(modern look) source code for creating network application in c++.

# http
I have done couple of experimental results, (libevent-http_parser)[https://github.com/avble/libevent-cpp-samples/tree/main/http], (libuv-http_parser)[https://github.com/avble/http_parser-libuv], asio-http_parser. 

All of them are performant, and I have personally decided to select asio as IO event and http-parser as parsing http request. 

## [http server example](https://github.com/avble/av_connect/example)

you can quick start creating a http server by the below source code.

``` cpp
int main(int argc, char ** argv)
{
    std::string addr = "127.0.0.1";
    uint16_t port    = 12345;
    http::start_server(port, [](http::response res) {
        res.body() = "[start_server_ng] hello";
        res.send();
    });
}
```

# websocket echo server

``` cpp
int main(int argc, char * args[])
{
    if (argc != 3)
    {
        std::cerr << "\nUsage: " << args[0] << " address port\n" << "Example: \n" << args[0] << " 0.0.0.0 12345" << std::endl;
        return -1;
    }

    std::string addr(args[1]);
    uint16_t port   = static_cast<uint16_t>(std::atoi(args[2]));
    auto ws_server_ = ws::make_server(
        port,
        [](ws::message msg) {
            msg.data_out() << "Echo: " << msg.data();
            msg.send();
        },
        [](beast::error_code ec) {});

    io_context::ioc.run();
}
```
# Performance
There are several criterias for measuring the performance of a http server.

This experimental result for av_http is conducted base on environment. And it looks impressive.
* OS: Linux 5.15.153.1-microsoft-standard-WSL2
* Hardware: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz

The ab tool is used to measure the performance. It is also understood that the evaluation result may vary on the environment (OS and hardware). 

## Request per second
This criteria is to measure the responsive of a http server.
It is tested by 50 concurrency active connection and 100,000 request in total.
``` shell
$ ab -k -c 50 -n 100000 127.0.0.1:12345/route_01
```

| http server | Request per second | Remark |
|----|----|---|
| av_connect  |      ~200,000 rps      |  release-0.0.4 |
| av_connect  |      ~155,000 rps      |  release-0.0.3 |
| nodejs   |    ~12,000 rps  | v12.22.9 |
| asiohttp | ~11,000 rps | 3.10.6 |
| flask   | ~697 rps | 3.0.3 |


Comparing with other http framework, which is found at [here](https://github.com/avble/av_http/example/performance)

## Concurrency
[C10K](https://en.wikipedia.org/wiki/C10k_problem) is used to be a problem to handle a large number of client at the same time.


``` shell
$ ab -k -c 1000 -n 1000000 127.0.0.1:12345/route_01
```

|server | Concurrency Level | Request per second | Remark |
|----|----|---|---|
| av_http | 1,000 | ~146,000 rps | release-0.0.3 |
| av_http | 5,000 | ~124,000 rps | release-0.0.3 |
| av_http | 10,000 | ~119,000 rps | release-0.0.3 |
| av_http | 15,000 | ~102,000 rps | release-0.0.3 |
| av_http | 20,000 | ~90,000 rps | release-0.0.3 |

# History
+ Update experimental result for release-0.0.2
+ Update experimental result for release-0.0.3
+ Update experimental result for release-0.0.4
