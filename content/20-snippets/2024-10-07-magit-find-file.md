+++
title = "magit-find-file"
date = 2024-10-07
tags = ["emacs", "til", "git"]
+++

I came across the `magit-find-file` function today[^1]. The doc string says:

>View FILE from REV.
>Switch to a buffer visiting blob REV:FILE, creating one if none
>already exists.  If prior to calling this command the current
>buffer and/or cursor position is about the same file, then go
>to the line and column corresponding to that location.

Up until now, I was always switching branches whenever I needed to peek
into a file that hadn't been merged yet. This is gonna make it quicker 
and easier to do that.

For example, to visit a file on a branch:

1. invoke `magit-find-file` in a repo
2. select the branch from the completion list
3. select the file

This works with remote branches too, which is great because github's PR
UI is a mess when there are many comments.

Additionally, there also exist the usual companion "find file" functions:

1. `magit-find-file-other-frame`
2. `magit-find-file-other-window`

I've not decided on a key binding but it'll probably be something under
the projectile keymap.


[^1] <https://www.reddit.com/r/emacs/comments/1fxol2u/working_with_multiple_branches_using_magit/>