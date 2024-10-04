+++
title = "Go unique package"
weight = 10
date = 2024-10-04
tags = ["go", "pl"]
+++

Phuong Le over at Victoria Metrics has a [blog post](https://victoriametrics.com/blog/go-unique-package-intern-string/) 
about the `unique` package in the Go standard library. I wasn't aware of the package so I'm grateful to learn about this.

If you didn't know about VictoriaMetrics, here's what Phuong says about themselves. They're also a Go shop.

>If you want to monitor your services, track metrics, and see how everything performs, you might want to check out VictoriaMetrics. It’s a fast, open-source, and cost-saving way to keep an eye on your infrastructure.

This is also the first time I heard the term [string interning](https://en.wikipedia.org/wiki/String_interning). I was aware of the behaviour in python. Once I discovered
that python kept a global cache of certain numbers to save memory. But I didn't know what it was called.

This is what I mean. 

```text
>>> a = 10
>>> b = 10
>>> a is b
True
>>> a = 100000
>>> b = 100000
>>> a is b
False
>>> 
```

I also learned that Python has a list of integers [ranging from -5 to 256](https://www.codesansar.com/python-programming/integer-interning.htm) in a global cache. 

There's a linked post that goes in more depth about [string interning in time series database](https://victoriametrics.com/blog/tsdb-performance-techniques-strings-interning/). I've added it to my to-read list.

