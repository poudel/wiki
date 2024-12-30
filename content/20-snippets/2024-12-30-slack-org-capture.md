+++
title = "org slack messages"
date = 2024-12-30
description = "Srijan Choudhary's convenient workflow from slack to org"
tags = ["emacs"]
+++

[Srijan Choudhary](https://srijan.ch) has a very interesting [blog
post](https://srijan.ch/capturing-slack-messages-directly-into-emacs-orgmode-inbox)
describing his workflow to invoke org capture from within slack.

Like Srijan, I've also been frustrated with the friction of capturing
input from other programs. This is an excellent way to reduce one of
those.

His new capture workflow is made possible by running Slack in the
browser[^fn:1] and a userscript to inject a button on the hover menu
of a slack message. The button is hooked up to Emacs via the org
protocol.

The [org
protocol](https://orgmode.org/worg/org-contrib/org-protocol.html) is a
sort of rpc that ships with org-mode. I found it difficult to get it
working the last time I tried it. Srijan, however, didn't seem to have
any problem.

Srijan is also using `prot-window-popup-frame` to open an
`emacsclient` frame to complete the capture. The frame is closed once
the capture is done.


[^fn:1] I also prefer running slack in the browser
