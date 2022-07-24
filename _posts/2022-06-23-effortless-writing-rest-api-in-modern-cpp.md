---
layout: post
title:  "Effortless of writing rest api in the modern C++"
date:   2022-06-23 14:14:45 +0700
categories: c/c++
---

This post aims to provide a simple way of writing Restful API on the top of Boost.Beast.

{% highlight c %}
beast_rest::route rs;

auto h1 = [](std::shared_ptr<beast_rest::request> req){
    req.get()->res_.body() = "this is response from handle 01";
    req.get()->res_.prepare_payload();
};

rs.add(http::verb::get, "/hello", std::move(h1));

// Create and launch a listening port
auto app = std::make_shared<beast_rest::rest_app>(
    ioc,
    tcp::endpoint{address, port},
    rs);

app->run();
{% endhighlight %}

The entire source code that hides the complexity of Boost.Beast is found [here](https://github.com/avble/boost_examples/tree/main/examples/beast_http/include/beast_rest)

An example of rest api is found [here](https://github.com/avble/boost_examples/blob/main/examples/beast_http/examples/main.cpp)