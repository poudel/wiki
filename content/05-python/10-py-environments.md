+++
title = "Python environments"
weight = 10
date = 2023-10-01
+++

## Pyenv


See all installed and available versions

```sh
pyenv versions
```


To list all installation options that are available to us.

```sh
pyenv install -l | grep \ \ 3
```

The `\ \ 3` gives me only the version 3 ones. The output contains two spaces
on each line, hence the escaped spaces `\ \ `.


To install a version, just run

```sh
pyenv install <full-version>
```


To set the local python version for the current directory, run:

```sh
pyenv local 3.11.1
```

This will create/update a `.python-version` file in the current directory.
