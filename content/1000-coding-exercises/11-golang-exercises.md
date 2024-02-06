+++
title = "Golang exercises"
weight = 11
+++

I'm trying to sharpen my Golang knowledge for $WORK. To do so, 
I've been building small one-off projects whenever I get a few 
hours free time. I'm also taking a lot of assistance from ChatGPT.

## SAML metadata parser

I was struggling with coming up with a good exercise when this idea occurred 
to me. One of the goals was to get myself familiar with writing tests in Go.
This turned out to be the perfect exercise for that. I also got to play with
[https://github.com/beevik/etree](beevik/etree) library for this. I'm sure
simpler XMLs can be directly parsed using the built in encoding package as well.

Later down the line, I got curious about how the module system works and how
packages are published in the open. So I decided to make this into a [published
library](https://github.com/poudel/samlutils/).

This also uses a github action to build and run tests when changes are merged
to the main branch. Of course, this is not a very useful library. It's only an
experiment.

## Websocket server

The idea behind this was to get myself comfortable with channels. The goal is 
very simple: use go channels and [websocket](https://github.com/gorilla/websocket)
to continuously update something on the browser.

Initial version has a websocket handler that wrote the current time every
second to the client. I started replaced that with currently available RAM using
[gopsutil](github.com/shirou/gopsutil/v3).

The package is [here](https://github.com/poudel/go-omnibus/tree/main/systeminfo).