+++
title = "Automating creating new entries"
date = 2025-02-02
tags = ["meta", "emacs"]
draft = false
+++

Up until now, new entries would be created manually. The process goes
like this:

1. I visit a new file in the appropriate directory. The name of the
   file nowadays contain the date of creation and the slug. Do that by
   hand.
2. Open an old file to copy the frontmatter.
3. Paste the frontmatter on to the new file and modify the title and
   date. This is probably slower than writing it from scratch every
   time in hindsight.
4. Start writing the post.

As you can see, this is an annoying bit of friction. So on Feb 2nd
2025, I finally wrote a yasnippet for the frontmatter.


```text
# -*- mode: snippet -*-
# name: fm
# key: fm
# --
+++
title = "$1"
date = `(format-time-string "%Y-%m-%d")`
tags = [$2]
draft = ${3:false}
+++

$4
```

This is great but I was in the mood for more. I still needed to
manually type the slug and the date when creating the new file.

So, with some ChatGPT help and misdirection, I ended up with the
following snippet.

```lisp
(defvar 
  kp-wiki-proj-directory
  "~/Projects/wiki/"
  "Directory root of the wiki."
)

(defvar 
  kp-wiki-new-entry-title 
  ""
  "Set when creating a new wiki entry so that yasnippet can expand with it.")

(defun kp-wiki-new-entry ()
  "Create a new entry in the wiki"
  (interactive)
  (let* ((base-dir (file-name-concat kp-wiki-proj-directory "content/"))
         (subdirs (seq-filter #'file-directory-p (directory-files base-dir t "^[^.]+")))
         (subdir (concat (completing-read "Choose subdirectory: " subdirs nil t) "/"))
         (kp-wiki-new-entry-title (read-string "Title: "))
         (slug (replace-regexp-in-string "[^a-z0-9]+" "-" (downcase kp-wiki-new-entry-title)))
         (filename (concat subdir (format-time-string "%Y-%m-%d") "-" slug ".md")))
    (find-file filename)
    (markdown-mode)
    (yas-expand-snippet (yas-lookup-snippet "fm"))
    ))
```

This function prompts me for a directory and the title of the new
entry. It then visits the new file as a markdown buffer. The file name
contains the iso date and the slugified title.

The buffer also opens with a yasnippet expansion of `fm` snippet which
contains the front matter. I could've just pasted the frontmatter in
Elisp, but yasnippet is more powerful because it lets me use
placeholders with defaults.

```text
# -*- mode: snippet -*-
# name: fm
# key: fm
# --
+++
title = "${1:`(format "%s" kp-wiki-new-entry-title)`}"
date = `(format-time-string "%Y-%m-%d")`
tags = [$2]
draft = ${3:false}
+++

$4
```

This is what the snippet looks like now. At first, I tried to pass the
`title` to `yas-expand-snippet` to replace `$1` but it didn't work.

So I went with the `kp-wiki-new-entry-title` variable and evaluate it
in the snippet like this.

This is so convenient, and I should've done this sooner.
