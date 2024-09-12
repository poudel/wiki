+++
title = "Upgrade Postgres"
weight = 100
+++

## Upgrading using pg_upgrade on MacOS

I'm using Postgres.app and I'm  not sure what's the easiest way for that is. After some trial and error,
this is what worked for me. I'm upgrading from 11 to 14.

### Initialise the new database

Shut the old server down and create a new server from Postgres.app's sidebar.

Don't initialise the new server yet. Initialising from the app created an extra user that pg_upgrade later complained about.
Instead, run the following command

```bash
/Applications/Postgres.app/Contents/Versions/14/bin/initdb -D /Users/keshp/Library/Application\ Support/Postgres/var-14 -U postgres --encoding=UTF-8 --locale=en_US.UTF-8
```

Run the `initdb` directly from the command line with these options. Keep in mind that if your old database was initialised with other options, they should be copied here as well.
In my case, this is the documented incantation in the Postgres app's readme. Don't run the new server yet.

### Upgrade

Run the following command to check if everything's alright and potential upgrade is possible

```bash
/Applications/Postgres.app/Contents/Versions/14/bin/pg_upgrade --check --old-datadir "/Users/keshp/Library/Application Support/Postgres/var-11/" --old-bindir "/Applications/Postgres.app/Contents/Versions/11/bin" --new-datadir "/Users/keshp/Library/Application Support/Postgres/var-14" --new-bindir "/Applications/Postgres.app/Contents/Versions/14/bin" -U postgres
```

This `--check` flag ensures a dry run and will flag any error that might come up during the actual upgrade. 

Without the `-U postgres` flag, pg_upgrade seems to pick th `$USER` by default, and I received the following message

```txt
-----------------------------
Checking cluster versions                                   ok
Checking database user is the install user                  
database user "keshp" is not the install user
Failure, exiting
```

Passing an explicit `-U postgres` as the upgrade user shut that down.

Also, I recieved following error when the first time when I'd initialised the database from the Postgres.app.

```txt
Performing Consistency Checks
-----------------------------
Checking cluster versions                                   ok
Checking database user is the install user                  ok
Checking database connection settings                       ok
Checking for prepared transactions                          ok
Checking for system-defined composite types in user tables  ok
Checking for reg* data types in user tables                 ok
Checking for contrib/isn with bigint-passing mismatch       ok
Checking for user-defined encoding conversions              ok
Checking for user-defined postfix operators                 ok
Checking for tables WITH OIDS                               ok
Checking for invalid "sql_identifier" user columns          ok
Checking for presence of required libraries                 ok
Checking database user is the install user                  
Only the install user can be defined in the new cluster.
Failure, exiting
```

It was clear after I ran `SELECT rolname,oid FROM pg_roles;` that the app created a role for `$USER` as well and `pg_upgrade` didn't like that. With those errors out of the way,
I ran the above command sans `--check` and received following.

```txt
Performing Consistency Checks
-----------------------------
Checking cluster versions                                   ok
Checking database user is the install user                  ok
Checking database connection settings                       ok
Checking for prepared transactions                          ok
Checking for system-defined composite types in user tables  ok
Checking for reg* data types in user tables                 ok
Checking for contrib/isn with bigint-passing mismatch       ok
Checking for user-defined encoding conversions              ok
Checking for user-defined postfix operators                 ok
Checking for tables WITH OIDS                               ok
Checking for invalid "sql_identifier" user columns          ok
Creating dump of global objects                             ok
Creating dump of database schemas
                                                            ok
Checking for presence of required libraries                 ok
Checking database user is the install user                  ok
Checking for prepared transactions                          ok
Checking for new cluster tablespace directories             ok

If pg_upgrade fails after this point, you must re-initdb the
new cluster before continuing.

Performing Upgrade
------------------
Analyzing all rows in the new cluster                       ok
Freezing all rows in the new cluster                        ok
Deleting files from new pg_xact                             ok
Copying old pg_xact to new server                           ok
Setting oldest XID for new cluster                          ok
Setting next transaction ID and epoch for new cluster       ok
Deleting files from new pg_multixact/offsets                ok
Copying old pg_multixact/offsets to new server              ok
Deleting files from new pg_multixact/members                ok
Copying old pg_multixact/members to new server              ok
Setting next multixact ID and offset for new cluster        ok
Resetting WAL archives                                      ok
Setting frozenxid and minmxid counters in new cluster       ok
Restoring global objects in the new cluster                 ok
Restoring database schemas in the new cluster
                                                            ok
Copying user relation files
                                                            ok 
Setting next OID for new cluster                            ok
Sync data directory to disk                                 ok
Creating script to delete old cluster                       ok
Checking for extension updates                              notice

Your installation contains extensions that should be updated
with the ALTER EXTENSION command.  The file
    update_extensions.sql
when executed by psql by the database superuser will update
these extensions.


Upgrade Complete
----------------
Optimizer statistics are not transferred by pg_upgrade.
Once you start the new server, consider running:
    /Applications/Postgres.app/Contents/Versions/14/bin/vacuumdb -U postgres --all --analyze-in-stages

Running this script will delete the old cluster's data files:
    ./delete_old_cluster.sh
```

I took the advice given and also ran the `vacuumdb` command. It wasn't necessary but still.

## Extensions

`pg_upgrade` creates `update_extensions.sql` file if it finds any extension. Just run the commands.

Also, run `./delete_old_cluster.sh` after ensuring that the new cluster is working well.

## Update the $PATH

Had a problem because I was inadvertently pointing to Pg 11 instead of 14.

```
sudo mkdir -p /etc/paths.d &&
echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```

Copied from <https://postgresapp.com/documentation/cli-tools.html>

