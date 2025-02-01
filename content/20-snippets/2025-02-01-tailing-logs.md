+++
title = "tailing logs"
date = 2025-02-01
tags = ["emacs"]
+++

I came across /u/asboans asking[^fn:1] for tips on following logs in Emacs.
And it occurred to me that I don't tail logs that often these days.
This is due to using tools like Sentry, and Datadog at $work. 

But that's not to say that tailing the logs in the terminal has gone
the way of the dodo, or that it is no longer necessary. Just that it's
not something I've had to do in a long time.

The poster is asking for tips on following logs inside emacs, and also
filtering them. As I said, I don't have to use `tail` right now, but
even when I had to in the past, I never thought of reaching to Emacs
for that. My habit has been to always use the termainal like `vterm`
and tail the log like this.

```shell
tail ../path/to/logs/*.log
```

A commenter suggests using `auto-revert-tail-mode`[^fn:2] which is
apparently similar to auto-revert-mode. This is built in and a
preferable one for me to try.

But how is `auto-revert-mode` different to this? From what I could
gather in the docs, `-tail-mode` sibling seems to be tailored for
files that are appended at the bottom. So it won't revert the whole
buffer, just tails it like `tail -f`.

One thing to note is that the buffers are like any other emacs buffers
so the regular cautions with opening large files in emacs still
applies.

Another user recommended using the logview[^fn:3] package. Looks very
featureful. Originally designed to target log files by java programs
like Log4j, but has no java specific requirement as long as the logs
follow similar format. I didn't try it myself but this seems useful
if you're in need of a proper log viewer.

## Links

1. [^fn:1] https://www.reddit.com/r/emacs/comments/1if44u0/using_emacs_to_tail_logs/
2. [^fn:3] TIL, I only knew about auto-revert-mode
3. [^fn:2] https://github.com/doublep/logview
