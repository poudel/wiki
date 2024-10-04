+++
title = "Replacing rich-minority with minions"
date = 2023-03-16
tags = ["blog", "emacs", "til"]
+++

Up until last week I'd been using rich-minority to curb the mode line
clutter that many emacs users experience. I must've installed it a
couple of years ago and I found it very useful. It did what I wanted
it to do and stayed out of my way. So there was no need to touch it.

This is what the configuration looked like:

```lisp
(use-package rich-minority
  :config
  (rich-minority-mode 1)
  (setq
   rm-blacklist
   (format
    "^ \\(%s\\)$"
    (mapconcat #'identity
               '("Fly.*"
                 "Projectile.*" "PgLn" "EditorConfig"
                 "Helm" "GitGutter" "Pipenv" "Elpy"
                 "Undo-Tree" "SP" "Abbrev")
               "\\|"))))
```


## minions {#minions}

I discovered `minions`[^fn:1] while
browsing through tarsius' github. It was no accident, I was hoping to
find interesting things there.

```lisp
(use-package minions
    :config (minions-mode 1))
```

That's it! Now only the name of the major mode is displayed. All minor
modes are tucked away in a menu that looks like `;-)`[^fn:2]. You can click on it or use the command
`minions-minor-modes-menu` to access the menu. And this was also when
I learned the meaning of this mysterious `;-)` emoticon[^fn:3] in several youtubers' mode lines.

[^fn:1]: <https://github.com/tarsius/minions/>
[^fn:2]: I know the
    parenthesis was already there
[^fn:3]: I'm not
    that old