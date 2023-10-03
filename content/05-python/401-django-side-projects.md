+++
title = "Starting side projects with Django"
weight = 401
date = 2023-10-01
+++

## Starter template

I use `django-admin startproject` command for most of my projects
because usually they're simple, and one-off. Otherwise, my go-to is
the excellent `cookiecutter-django` starter template, which takes care
of many things. However, I find `cookiecutter-django` to be quite
overkill sometimes.

I've also recently discovered
[sidewinder](https://stribny.github.io/sidewinder/), another starter
template. This is another interesting option for me. It is slightly
more 'minimal' compared to `cookiecutter-django`.

## Setting up postgresql

I use the excellent [Postgres.app](https://postgresapp.com/) on MacOS.
For ArchLinux, I just use the one in the repositories. In servers, I
just pick the one from the official repositories. But the following
should work for any one of these.

When I'm starting a new project, I'm usually doing this on my macbook.
Accessing the `psql` shell is as easy as opening the application and
double clicking on one of the databases. I double click on the
`postgres` database, and am presented with a psql shell on a `Terminal` window.

This is where I run the following to create a new database and setup a role.

```sql
CREATE DATABASE <db_name>;
```

Where `<db_name>` is the name of the new database, of course. The following,

```sql
CREATE USER <db_name> WITH PASSWORD '<db_name>';
GRANT ALL PRIVILEGES ON DATABASE <db_name> TO <db_name>;
```

creates a new user/role with the same name and grants all permissions
on that database.

## Error while installing psycopg2

I forgot to put postgresapp on the path the first time. Which made it
throw an error when installing `psycopg2`. Following was taken from
[postgresapp.com](https://postgresapp.com/).


```shell
sudo mkdir -p /etc/paths.d &&
echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```
