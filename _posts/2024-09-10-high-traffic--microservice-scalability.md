---
layout: post
title:  "An http server based on non-blocking socket for efficiency"
---


# Introduction
Non-blocking socket is emerging technique used in various well-known http framework these days.
To name a few, nodejs (javascript), aiohttp (python), boost-beast (C++), boost-asio (C++)

[libevent](https://https://github.com/libevent/libevent) library is very popular in utilizing non-blocking socket.
And it is widely used in several [application](https://libevent.org/).

I have created [mini-http server](https://github.com/avble/av_http/) from scratch by based on the libevent. 


av_http presents an intutive way(modern look) source code for creating a http server.
In addition, an experiment on performance is also exercised. Its result is more impressive than my expectation.

# [av_http](https://github.com/avble/av_http/)

you can quick start creating the http server by the below source code.

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

Or you can start writing a simple router which is to dispatch your request to appropriate handler.
As below exapmle, two route handler are created, /route_01 and /route_02

```cpp
int main(int argc, char ** argv)
{
    std::string addr = "127.0.0.1";
    uint16_t port    = 12345;
    std::unordered_map<std::string, std::function<void(http::response)>> routes;

    routes["/route_01"] = [](http::response res) {
        res.body() = "hello from route_01";
        res.send();
    };
    routes["/route_02"] = [](http::response res) {
        res.body() = "hello from route_02";
        res.send();
    };

    auto route_handler = [&routes](http::response res) {
        if (auto route_ = routes[res.reqwest().get_uri_path()])
            route_(std::move(res));
        else
            res.send();
    };

    http::start_server(port, route_handler);
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
| av_http  |      ~155,000 rps      |  release-0.0.3 |
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
