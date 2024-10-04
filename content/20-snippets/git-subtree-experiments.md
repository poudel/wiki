+++
title = "Experiments with git-subtree"
date = 2022-08-14
tags = ["git", "emacs"]
+++

I'm learning the Rust language these days. I like making real things
once I get a little comfortable with the language. One of the first
things is to make a basic text editor.

For this, I'm following [Hecto: Build your own text editor in Rust](https://www.philippflenker.com/hecto/) by
Philipp Flenker. It's a well written tutorial and I'm thoroughly
enjoying it.

Then somehow I find myself watching John Wiegley's [It's Magit!](https://www.youtube.com/watch?v=j-k-lkilbEs) talk on
youtube. It's an excellent introduction to magit, by the way. I've
been using it for many years now and I still discover new ways to
improve my workflow.

At this point, I'm overcome with an overwhelming sense to tinker with
something. And, here we are. It even got an excuse to write a post
after a long time.

This is written in org-mode and published with org-mode. It is a
github repository with it's own build/publish flow using emacs and a
`Makefile`.

That slightly changes from today. Now the repo is a "subtree" in my
dotemacs directory. The elisp configuration also becomes a part of my
emacs config.

It'll also keep staying as a separate repository on github. This is
partly because github only lets you publish one page per page.
Actually, I think it's mostly that.


## Why am I doing all this? {#why-am-i-doing-all-this}

I admit it's mostly because I needed to tinker with something.

However, there are some technical benefits to this. The first is to
have all my elisp in one place. The plumbing required to get this site
published is not a significant one but I can reuse some of the code to
publish other sites and so on.

It's also a step into my ongoing work to consolidate all my text files
in one repo. This will include dotfiles, of course, but also files
that I want to carry with me to the grave. When I'm done, everything
will be part of that one big repository.


## How does it work? {#how-does-it-work}

It's simple, really. You add the repo as a subtree. Subtree is just
another way of saying a directory in the git repo. It's fundamentally
different to `submodules` in a sense that submodule references another
git repository while subtree blatently copies it into yours.

This has a major benefit for my type of situations. I can mainly work
from my main repo: making changes to the subtree as a first class
citizen of your project. It's more suited for this because the changes
will always flow from my main repo to the sub-repo because I only ever
push there to publish, and I'm the sole person to make changes.

Obviously, it's possible to have changes flow in both direction, but
it's just not something I need.

I wrote a couple of elsip functions to build and publish the site.
This one is to build the site and copy the `CNAME` file if it doesn't
exist already.


### Namespacing mishap {#namespacing-mishap}

I tried namespacing the component names with a `kes/` prefix but that
raised an error while trying to generate the website because
`ox-publish` tries to create caches to avoid regenerating old files.

It does so by creating a directory called
`.org-timestamps/<file-path>`.

Since my original prefix had a forward slash in it, it complained that
the directory `.org-timestamps/.../kes/` doesn't exist. So I just
changed the prefix.


### TIL {#til}

In a magit buffer, you can use 1, 2, 3, 4 keys to show diff/patches
instead of hitting the TAB key multiple times.


## Appendix {#appendix}


### Elisp code to build and publish keshp.com {#elisp-code-to-build-and-publish-keshp-dot-com}

Update: 2022-08-27

Previously this was an org include link to the function in my init
file but any small change on that file made this regenerate while
building the site. So, now it's just copied here. See the `dotemacs`
directory for an up to date version.


#### Basics {#basics}

```emacs-lisp
(defun kes/expand-keshp.com (path)
  "Expand to absolute path inside ./keshp.com/ subtree"
  (expand-file-name (concat "~/dotfiles/keshp.com/" path)))

(defconst gh-poudel "git@github.com:poudel/")
(defconst keshp-com-subtree "keshp.com")
(defconst gh-keshp-com (concat gh-poudel keshp-com-subtree ".git"))
```


#### Configure org-publish {#configure-org-publish}

First we configure  `org-publish-project` by setting
`org-publish-project-alist`.

```emacs-lisp
(setq org-html-validation-link nil)
(setq org-publish-project-alist
  `(("keshp.com-home"
    :base-directory ,(kes/expand-keshp.com "src")
    :base-extension "org"
    :publishing-directory ,(kes/expand-keshp.com "docs")
    :recursive nil
    :publishing-function org-html-publish-to-html)

   ;; base directory src to preserve the relative path
   ("keshp.com-static"
    :base-directory ,(kes/expand-keshp.com "src")
    :base-extension "css\\|js\\|png\\|svg\\|txt"
    :publishing-directory ,(kes/expand-keshp.com "docs")
    :publishing-function org-publish-attachment
    :recursive t)

   ("keshp.com-blog"
    :base-directory ,(kes/expand-keshp.com "src/blog")
    :base-extension "org"
    :publishing-directory ,(kes/expand-keshp.com "docs/blog")
    :recursive t
    :publishing-function org-html-publish-to-html

    :section-numbers nil
    :with-creator t
    :with-date t
    :with-toc nil

    :html-head nil
    :html-head-include-default-style nil
    :html-head-extra "<link rel='stylesheet' href='../static/index.css' />"
    :html-doctype "html5"

    :auto-sitemap t
    :sitemap-title "blog"
    :sitemap-filename "index.org"
    :sitemap-style list
    :sitemap-sort-files anti-chronologically
    )

   ("keshp.com-website"
    :components ("keshp.com-blog" "keshp.com-home" "keshp.com-static")
    :html-validation-link nil
    )))
```


#### Build the site {#build-the-site}

```emacs-lisp
(defun kes/build-keshp-com ()
  "Build keshp.com using org-publish into the /docs directory"
  (interactive)
   (org-publish-project "keshp.com-website")
   (copy-file (kes/expand-keshp.com "CNAME") (kes/expand-keshp.com "docs/CNAME") t t))

;;  (kes/build-keshp-com)
```


#### Push the subtree {#push-the-subtree}

The following function pushes any new commits for files with the
prefix `keshp.com` (i.e. inside `keshp.com/` directory) to it's own
github repo.

```emacs-lisp
(defun kes/push-keshp-com ()
  "Push keshp.com subtree changes to remote"
  (interactive)
  (magit-subtree-push keshp-com-subtree gh-keshp-com "master" nil))
```


## Links {#links}

1.  [Git subtree basics](https://gist.github.com/SKempin/b7857a6ff6bddb05717cc17a44091202) gist
2.  <https://github.com/poudel/dotemacs/>
3.  <https://github.com/poudel/keshp.com>
4.  <https://magit.vc/manual/magit/Subtree.html>