+++
title = "Python environments"
weight = 10
date = 2023-10-01
+++

## Pyenv


See all installed and available versions

```shell
pyenv versions
```


To list all installation options that are available to us.

```shell
pyenv install -l | grep \ \ 3
```

The `\ \ 3` gives me only the version 3 ones. The output contains two spaces
on each line, hence the escaped spaces `\ \ `.


To install a version, just run

```shell
pyenv install <full-version>
```


To set the local python version for the current directory, run:

```shell
pyenv local 3.11.1
```

This will create/update a `.python-version` file in the current directory.
