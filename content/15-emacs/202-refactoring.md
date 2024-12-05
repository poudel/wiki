+++
title = "Config refactoring"
weight = 202
date = 2024-11-30
+++

Over the years, I've grown my config into a respectable
size. It used to be public on github but I've made it private
now.

The config is a big `.org` file that I've come to love and hate
at the same time. It's time I refactor it, disable and remove
packages that I have no use for. I also want to develop
better habits but that's for another time.


# Changes

## Disable flycheck

Disabling it because I am not doing a lot of coding in Emacs
right now. Using Pycharm and Goland. Might enable it later though.

```lisp
(use-package flycheck
  :hook
  ((go-mode . flycheck-mode)
   (js2-mode . flycheck-mode)
   (yaml-mode . flycheck-mode)))
```

## Remove smartparens

I used to use `C-c s a` and `C-c s e`. But that feels like an eternity ago.

```lisp
(use-package smartparens
  :hook
  ((prog-mode . smartparens-mode)
	 (markdown-mode . smartparens-mode))
  :config
  (sp-local-pair 'web-mode "{" "}" :actions nil)
  :bind
  (("C-c s a" . sp-beginning-of-sexp)
   ("C-c s e" . sp-end-of-sexp)
   ("M-[" . sp-backward-unwrap-sexp)
   ("M-]" . sp-rewrap-sexp)
   ("C-M-f" . sp-forward-sexp)
   ("C-M-b" . sp-backward-sexp)))
```

## Remove yafolding

Not using it a lot these days. Besides, I don't really fold
code that much nowadays. I guess the habit was from my early days.

```lisp
(use-package yafolding
  :hook (prog-mode . yafolding-mode))
```

## Remove multiple cursors

Don't remember the last time using it...

```lisp
(use-package multiple-cursors
  :bind
  (("C->" . mc/mark-next-like-this)
   ("C-<" . mc/mark-previous-like-this)
   ("C-c C-<" . mc/mark-all-like-this)
   ("C-S-c C-S-c" . mc/edit-lines)))
```

## Remove projectile and consult-projectile

The only two things I'm using it for are going to a 
project and finding files inside a project. 


```
(use-package projectile
  :init
  (projectile-mode)
  :bind-keymap
  ("s-f" . projectile-command-map)
  :config
  (setq projectile-enable-caching t
        projectile-switch-project-action #'projectile-find-file)
  (progn
    (add-to-list 'projectile-globally-ignored-files "node-modules")
    (add-to-list 'projectile-globally-ignored-files "venv")))


(use-package consult-projectile
  :after consult
  :bind
  (("s-w" . consult-projectile)
   (:map projectile-command-map 
        ("f" . consult-projectile-find-file))))
```

There's a `consult-project-extra` package if I need it but for 
now, it's bare minimum. 

```lisp
(use-package project
  :bind-keymap
  ("s-f" . project-prefix-map))
```

## remove easy-kill

Don't think I'm using these at all. I rely on the default `M-w` and
`C-w` behaviour, and don't expect anything more there. So it goes to
the bin.

```lisp
(use-package easy-kill
  :bind
  (([remap kill-ring-save] . easy-kill)))
```

## remove which-key

I think which-key is an excellent package, and it definitely 
helps with discovery...but only if you're looking to discover
new commands. 

```
(use-package which-key
  :config
  (which-key-mode))
```

## remove dwim-shell-command

I was really excited when I discovered this last year. But
turns out I didn't really use it that often. Maybe I'm not
doing this right. For now, this gets removed.


## disable git-gutter

I'll probably revert this back. 

Update: Yes, reverted it back.

## remove forge (magit)

Not using this.

## remove eglot

This is slowing everything down. I'll probably setup one of
the bridge lsp servers later.

```lisp
(use-package eglot
  :hook ((python-ts-mode . eglot-ensure)
         (zig-mode . eglot-ensure)
         (rust-ts-mode . eglot-ensure))
  :bind-keymap ("C-c l" . eglot-mode-map)
  :bind (:map eglot-mode-map
              ("C-c l r" . eglot-rename)
              ("C-c l o" . eglot-code-action-organize-imports)
              ("C-c l h" . eldoc)
              ("C-c l c" . eglot-reconnect)
              ("C-c l a" . eglot-code-actions))
  :config
  (setq eglot-ignored-server-capabilities '(:inlayHintProvider))
  (setq eglot-extend-to-xref t)
  (add-to-list 'eglot-stay-out-of 'flymake)
  (add-to-list 'eglot-stay-out-of 'eldoc))
```

## remove dash for docs

This was marked for removal before

```lisp
(use-package dash-docs
  :config
  (setq dash-docs-browser-func 'eww)
  (setq dash-docs-common-docsets '("PostgreSQL")))

(defun kes-python3-docs ()
  (interactive)
  (setq-local dash-docs-docsets '("Python 3" "Django" "PostgreSQL")))

(add-hook 'python-mode-hook 'kes-python3-docs)

(use-package consult-dash
  :after consult
  :bind (("C-c d" . consult-dash))
  :config
  (consult-customize consult-dash :initial (thing-at-point 'symbol)))
```

## remove treemacs

Wasn't using it.

## habit: switching windows

I have been using this for a long time now. 

```lisp
(use-package emacs
  :bind
  ("s-n" . next-multiframe-window)
  ("s-m" . previous-multiframe-window))
```

Due to the force of habit, I've not been able to switch to `ace-window`.
But I'm gonna force myself by removing these keybindings and switching
to `s-o`

```list
(use-package ace-window
  :bind ("s-o" . ace-window)
  :config
  (setq aw-scope 'frame)
  (set-face-attribute 'aw-leading-char-face nil :height 1.9))
```

Finally, doing this so that punishment for using the old bindings is
not harsh. MacOS binds them to New window and Minimize by default.

```lisp
(global-set-key (kbd "s-n") nil) ;; new window
(global-set-key (kbd "s-m") nil) ;; minimize
```

## Remove htmlize

I'm not using org to publish html these days so this is not necessary

```lisp
(use-package htmlize
  :config
  (setq org-html-htmlize-output-type 'css))
```

## Remove semantic

When editing certain files, every pause triggered a garbage collection. It only
happened in certain buffers once in a while. After going through every
minor modes on that buffer, I found the culprit to be `semantic-idle-scheduler-mode`. 

But I will switch off semantic mode instead of configuring this minor mode because 
I don't really need it. I don't know why I enabled it in the first place.

```lisp
(semantic-mode t)
```

By turning it off, I may have solved an older issue where every single keystroke
would take about half a second. 


## Spell checking

I've not bothered to set this up properly. Needs revisiting.

```lisp
(use-package ispell
  :config
  (setenv "DICTIONARY" "en_GB")
  (setq ispell-program-name "hunspell"
        ispell-dictionary "en_GB"
        ))

(use-package flyspell
  :config
  (flyspell-mode))
```

## Remove undo-tree

Using `vundo` now.

```lisp
(use-package undo-tree
  :defer t
  :init (global-undo-tree-mode)
  :custom
  (undo-tree-auto-save-history nil)
  (undo-tree-visualizer-diff t)
  (undo-tree-history-directory-alist '(("." . "~/.emacs.d/undo")))
  (undo-tree-visualizer-timestamps t))
```

# For later

While going through my config, I came across many other packages
and workflows. I get easily distracted by clicking on a dozen
links and reading everything...and that's no good. So here's
a list that future me can check.

## detached retry

Give [detached.el](https://sr.ht/~niklaseklund/detached.el/) another try.
I love the concept of it but I've not really used it in real life.

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

## undo-tree vs vundo

undo-tree is one of the most common package that I use but is vundo
better?

> Vundo doesn’t need to be turned on all the time nor replace the undo
>commands like undo-tree does. Vundo displays the tree horizontally,
>whereas undo-tree displays a tree vertically.  Diff is provided
>on-demand between any nodes.
<https://github.com/casouri/vundo>

also

>I have not used vundo, but it seems to be just a visualizer for the 
>buffer undo list, whereas undo-tree takes over the undo system, 
>replacing not just the commands but the data structure 
>(https://github.com/apchamberlain/undo-tree.el/blob/help-in-v...). 
>In other words, I think there's simply less surface area for vundo
>to have or cause problems.
<https://news.ycombinator.com/item?id=33057027>

Although I don't remember the last time I've had a problem with
undo-tree.

Update: switched to `vundo`


## the rest

* <https://github.com/emacscollective/no-littering>
* Reddit: Onatolin's [comment](https://www.reddit.com/r/emacs/comments/1dj4298/comment/l9f0cmc/) 
  on using Embark as an alternative to which-key
* Reddit: Which key is available in [Emacs 30](https://www.reddit.com/r/emacs/comments/1dj4298/whichkey_now_included_in_emacs_30/)
* give [eat terminal](https://codeberg.org/akib/emacs-eat) another try
* more dired-hacks, (currently I'm using `diredfl` only)
* try [verb](https://github.com/federicotdn/verb) to replace `restclient.el`
* spell checking setup
* compile command <https://www.reddit.com/r/emacs/comments/1h2o8hr/share_your_mx_compile_compilationmode_config/>
