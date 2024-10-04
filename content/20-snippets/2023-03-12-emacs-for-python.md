+++
title = "Back to using Emacs for Python"
date = 2023-03-12
tags = ["blog", "emacs", "python", "macos"]
+++

About two years ago I switched to Pycharm for my bread &amp; butter
necessicities. Before that, I was using Emacs. I had flirted with
Pycharm before but until then I'd never actually considered switching
for a number of reasons.


## I didn't want to switch to Pycharm {#i-didn-t-want-to-switch-to-pycharm}


### Old habits {#old-habits}

Being used to a keyboard and command driven workflow, the thought of
switching to what seemed like a more mouse driven environment also
wasn't very compelling. I knew that it was possible to setup pycharm
to be entirely driven by the keyboard but it's not the same.


### Performance {#performance}

Pycharm's reputation for hogging all available resources in your
machine is another reason. Despite running on a modern hardware, and
taking up as many gigs of RAMs as it pleased, I saw it still lag on
input from time to time.

Pairing with colleagues who used Pycharm was a test of patience
because it'd take half a second for the keystrokes to appear on the
screen. Typing commands over SSH into a server I had in a different
continent was faster than that.


### magit! {#magit}

I was, and still am, beholden to certain Emacs specific packages like
`magit`, and `restclient.el`. I have yet to find an equivalent for the
former, not that I'm trying to, but Pycharm paid version has a decent
substitute for the latter.


## Switching to Pycharm anyway {#switching-to-pycharm-anyway}


### Flirting with Pycharm {#flirting-with-pycharm}

All of the above didn't stop me from taking Pycharm for a spin from
time to time. I'd known for a while that Pycharm excelled at certain
things. Mainly these: debugging, locating references and refactoring.
This made Pycharm an appropriate tool to navigate unfamiliar
codebases.


### Not content with Elpy {#not-content-with-elpy}

I could never get my Emacs setup to give me as good and as consistent
result for navigating a codebase. Sometimes jump to definition
wouldn't work for things I knew were there but for some reason Elpy
couldn't see. That of course meant that I could never be sure if I
found all references when I was refactoring/removing something. I was
relient on the `dumb-jump` package because I couldn't get `xref` to
work.


### New job {#new-job}

When I joined Ben, I decided to give Pycharm the front row seat to my
day to day activities, and use it full time for a few days to see if
I'd miss Emacs. It turned out that I did, but only for `magit` and not
for my python editing needs.

Getting around a new project was a breeze with Pycharm. It allowed
me to not worry too much about my editor setup and get productive
very quickly. It also helped that the project was relatively new.

So I've been using it for python development in tandem with Emacs ever
since. I use Emacs for magit and certain ad-hoc things like keyboard
macros to manipulate text quickly.


## Switching back to Emacs {#switching-back-to-emacs}

A lot of the pain points of Pycharm stated above still stand to this
day. It works but I'm not fully satisfied with it. This is mostly why
I want to move it back to Emacs. I'm skeptical that it'll immediately
replace Pycharm but that's not exactly my goal right now.

I've noticed LSP (and `lsp-mode`) become more popular and mature from
the fringes. I had hard time getting it to work the first time I tried it a
couple of years ago. Hope that's not the case anymore.

With the help of lsp-mode, I have a feeling that I can use Emacs for
almost everything that I do on my day to day. If not, I still have
Pycharm to go back to.


## Expectations {#expectations}

So my needs are quite basic. But here's a list of things that I'd like
to have coming from Pycharm. I'm excluding things like find across
project that I know for sure exist on Emacs.

My work project is a Django monolith. We use a recent version of
Python. We use Postgres for database but I don't need the data related
tools that Pycharm provides since I don't use them.

My side projects might have slight variations in requirements here and
there but I can easily adapt them to suit the new workflow.


### Navigation {#navigation}

Support for "jump to definition" and "find where this function/class
is being used". Should be able to find installed dependencies, not
just local resources.


### Debugging {#debugging}

Ability to run and debug the programme is a must. I am still a print
debugger at heart but sometimes a proper debugger is simply more effective.
This is the


### Editing {#editing}

Should have a decent autocomplete support for local and installed
libraries.

I also want be able to format on save using `black` and sort imports
using either `ruff` or good old `isort`. I know that this is possible
however, I might opt to do this outside the editing flow and use
`pre-commit`.


## Steps {#steps}

Start by disabling or removing any existing IDE packages like `elpy`.
Call `M-x elpy-disable` to temporarily disable Elpy.

To get started, I had to choose the lsp implementation that I want.
This is the actual language server that parses the source code and
listens to "commands" from the editor. I decided to go with `pylsp`
because it has `dap` support; debugger is something I'd love to have.

Install `pylsp` with

```sh
pipx install python-lsp-server[all]
```

Then installing `lsp-mode` with `use-package` is as trivial as:

```lisp
(use-package lsp-mode
  :init
  (setq lsp-keymap-prefix "C-c l")
  :hook ((python-mode . lsp)) ;; enable lsp for python mode files
  :commands lsp)
```

You'd add more items inside `:hook` to add support for more languages.


## Conclusion {#conclusion}

So this setup is all ready to go. I'll see how it goes over the next
few weeks and report back.


## Links {#links}

-   <https://emacs-lsp.github.io/lsp-mode/>
-   <https://github.com/python-lsp/python-lsp-server>
-   <https://github.com/charliermarsh/ruff>
-   <https://github.com/charliermarsh/ruff-lsp>