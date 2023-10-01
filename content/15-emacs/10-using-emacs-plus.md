+++
title = "Emacs Plus"
weight = 10
+++

If you're using Emacs on MacOS, chances are you're using the one from
Emacs-For-Mac-OSX website. This makes getting it so easy because there
are no official binaries available for mac and you can be sure that
you're getting vanila emacs, without anything added or removed. That's
excellent.

The binary offered there is a little behind because it only tracks the
latest stable release, which is `28.2` at the moment. The bleeding
edge of emacs is at version `30`. The new version has a bunch of
improvements and some exciting new features.

So I decided that I wanted to build Emacs from scratch. While looking
for the easiest way to do it, I discovered
`d12frosted/homebrew-emacs-plus`[^fn:1], which is a hombrew recipe to easily
build Emacs from scratch. This recipe lets you throw in several flags
to tailor the build to your preference. The one that I really love is
the `--with-no-frame-refocus` flag. This stops existing Emacs frame
from stealing focus when you close another Emacs frame.

Another interesting one is the `--with-native-comp` flag. This adds
the ability to compile emacs lisp as native code in Emacs. Which
presumably improves performance. I'll try this some time in the future
when I have enough time.


## Installing {#installing}

Add the homebrew tap

```sh
brew tap d12frosted/emacs-plus
```

Then run:

```sh
brew install emacs-plus@30 --with-no-frame-refocus --with-imagemagick --with-xwidgets
```

This will download and build emacs from source so it can take a while.

After it's done, link it to the `/Applications` directory.

```sh
ln -s /opt/homebrew/opt/emacs-plus@30/Emacs.app /Applications
```

For some reason I wasn't able to launch Emacs from spotlight even
after linking it like this. I decided to start using emacsclient
instead.


## Using `emacsclient` {#using-emacsclient}

Start the homebrew service first using this command and connect to it
from the command line using `emacsclient -c`.

```sh
brew services start emacs-plus@30
```

It works like a charm. Emacs client loads up quite fast because it's simply
connecting to the running editor daemon.

I've avoided this type of setup for a while because there are certain
caveats to consider. For example, changing config and restarting
becomes a bit of a hassle, you can't just close and open Emacs. I have
a habit of restarting emacs if I change something that can't be loaded
with a simple `C-x C-e`. Example: immediately after removing an
`add-hook` function call. The hook will still be registered in memory
unless you manually remove it using `remove-hook` command or do a full
restart.

I also restart to ensure that I have a working setup the next time I start
Emacs (sometimes in a different computer after syncing config). This I can
test with a new Emacs instance by running `emacs` from he command line after
each config change. So it's not a big deal.

Another issue I ran into was that emacs daemon didn't load my
environment variables in my `.zshrc`. Which was slightly confusing
since I am using `exec-path-from-shell` package already. Turns out I
had it configured to only read environment variables when launching
Emacs directly like this.

```lisp
(use-package exec-path-from-shell
  :ensure t
  :init
  (when (memq window-system '(mac ns x)))
```

I changed it to always be invoked and that did the trick.

```lisp
(use-package exec-path-from-shell
  :ensure t
  :init
  (exec-path-from-shell-initialize))
```


## Launching from spotlight {#launching-from-spotlight}

Create a shortcut via Automator with the `Run Shell Script` task and
move the file to `/Applications` directory. The shell command should
look like this.[^fn:2]

```sh
/opt/homebrew/bin/emacsclient \
    --no-wait \
    --quiet \
    --suppress-output \
    --create-frame \
    "$@"
```

[^fn:1]: [d12frosted/homebrew-emacs-plus](https://github.com/d12frosted/homebrew-emacs-plus)
[^fn:2]: [Using Emacsclient on MacOS](https://blog.lambda.cx/posts/using-emacsclient-on-macos/)
