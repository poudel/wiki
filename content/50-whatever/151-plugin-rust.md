+++
title = "Plugin system for rust projects"
weight = 151
+++

I'm working on a Rust project that needs to be extensible
by the user.

My needs are specific. My users aren't always going to be programmers.
Asking them to write Rust or C++ is probably too much. So the plugin
system needs to simple enough to cater to these power users. Embedding
some sort of scripting language is probably the way to go.


## [Plugins in rust](https://nullderef.com/series/rust-plugins/)

A series of well written essays into reasearch and development of a
plugin system in Rust. After some careful research, the author decides
to use dynamic loading because it suits their need better than other
options, namely Webassembly.


## Links


* [Scotch](https://github.com/ItsEthra/scotch) (WASM plugin system)

