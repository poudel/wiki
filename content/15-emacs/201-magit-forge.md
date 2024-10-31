+++
title = "Magit Forge"
weight = 201
+++

Setup github username using

```sh
git config --global github.user <username>
```

Create PAT (Personal Access Token) from <https://github.com/settings/tokens> and store it inside `/.authinfo.gpg` like so:

```txt
machine api.github.com login <username>^forget password <token>
```

then set the auth source inside emacs config

```
(setq auth-sources '("~/.authinfo.gpg"))
```
