---
layout: post
title:  "uriparser in the modern C++"
date:   2022-07-21 14:14:45 +0700
categories: c/c++
---
# Introduction
The uriparser is a strictly RFC 3986 compliant URI parsing and handling library written in C89 ("ANSI C")

This post shows how to use the uripaser for parsing the URI in the mordern C++

An example of uriparser in modern C++
{% highlight c %}
uriparser_wrap uri_parser_("http://127.0.0.1:12321?keya=valuea&&keyb=valueb");

std::cout << "scheme: \t" << uri_parser_.get_scheme() << std::endl;
std::cout << "host: \t\t" << uri_parser_.get_host() << std::endl;
std::cout << "port: \t\t" << uri_parser_.get_port() << std::endl;
std::cout << "uri params:" << std::endl;
for (auto &[key, value]: uri_parser_.m_query_)
    std::cout << "\t" << key << ":\t" << value << std::endl;

{% endhighlight %}

An example of uriparser in ASCII C
{% highlight c %}
// the example of uri parser in C
if (uriParseUriA(&state, "http://127.0.0.1:12321?keya=valuea&&keyb=valueb") != URI_SUCCESS) {
    return;
}

// The scheme is stored between .first and .afterLast
// (uri.scheme.first, uri.scheme.afterLast);
// and similar to host, port 

// Get params of query
UriQueryListA * queryList = NULL;
int itemCount = 0;
const int res = uriDissectQueryMallocA(&queryList, &itemCount,
        uri.query.first, uri.query.afterLast);

UriQueryListA *p_cur_query = queryList;
while (p_cur_query != NULL){
    // key: p_cur_query->key;
    // value: p_cur_query->value;
    p_cur_query = p_cur_query->next

}
// free memory
uriFreeQueryListA(queryList);
uriFreeUriMembersA(&uri);
{% endhighlight %}

The output on console
{% highlight bash %}
scheme:         http
host:           127.0.0.1
port:           12321
uri params:
        keya:   valuea
        keyb:   valueb
{% endhighlight %}


The full source code can be obtained from
[source code](https://github.com/avble/cpp_utils/blob/main/test/uriparser_wrap.hpp)



