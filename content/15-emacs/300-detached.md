+++
title = "Trying out detached.el"
weight = 300
draft = true
+++


[detached.el](https://sr.ht/~niklaseklund/detached.el/) uses
[detach](https://github.com/crigler/dtach) package to allow terminal
session management inside Emacs. Sessions like you might have with
tmux or screen, but inside Emacs.

I use the [vterm](https://github.com/akermu/emacs-libvterm) package
for almost all of my terminal. I also use Pycharm, and every now and
then, I need to use the integrated terminal. It would be great for
my workflow to be able to share the terminal session between these two.


I know it's possible to make tmux work inside Emacs but my needs are
far simpler. I don't use any other terminal, except the default Termainal.app
once in a while to restart emacs server.


# Installing

Copying the `use-package` invokation from the readme.

```lisp
(use-package detached
  :init
  (detached-init)
  :bind (;; Replace `async-shell-command' with `detached-shell-command'
         ([remap async-shell-command] . detached-shell-command)
         ;; Replace `compile' with `detached-compile'
         ([remap compile] . detached-compile)
         ([remap recompile] . detached-compile-recompile)
         ;; Replace built in completion of sessions with `consult'
         ([remap detached-open-session] . detached-consult-session))
  :custom ((detached-show-output-on-attach t)
           (detached-terminal-data-command system-type)))
```

We also need to install the `dtach` command. It's as simple as

```
nix-env -iA nixpkgs.dtach
```

