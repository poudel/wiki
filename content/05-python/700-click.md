+++
title = "Click"
weight = 700
+++

Click is a very convenient way to build
command line applications in python. Example:

```
import click


@click.command()
def hello():
    click.echo("Hello world")
    
hello()
```

This file just became a command line application.

## Options & Arguments

A click command can have [options and arguments](https://click.palletsprojects.com/en/stable/parameters/).

>**Options**
>Are optional.
>Recommended to use for everything except subcommands, urls, or files.
>Can take a fixed number of arguments. The default is 1. They may be specified multiple times using Multiple Options.
>Are fully documented by the help page.
>Have automatic prompting for missing input.
>Can act as flags (boolean or otherwise).
>Can be pulled from environment variables.

>**Arguments**
>Are optional with in reason, but not entirely so.
>Recommended to use for subcommands, urls, or files.
>Can take an arbitrary number of arguments.
>Are not fully documented by the help page since they may be too specific to be automatically documented. [...]
>Can be pulled from environment variables but only explicitly named ones. For more see Environment Variables.

It seems that options will be my go o 

## Packaging

Click has a [guide here](https://click.palletsprojects.com/en/stable/setuptools/#setuptools-integration)
on how to use setuptools to package the CLI. If you're distributing your package,
it's important that you don't rely just on the shebang. 

This is what the `setup.py` would look like:

```
from setuptools import setup, find_packages

setup(
    name='yourpackage',
    version='0.1.0',
    packages=find_packages(),
    include_package_data=True,
    install_requires=[
        'Click',
    ],
    entry_points={
        'console_scripts': [
            'yourscript = yourpackage.scripts.yourscript:cli',
        ],
    },
)
```

But, I want to avoid writing `setup.py`, and use `pyproject.toml`
instead. 
