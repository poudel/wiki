+++
title = "Saving history"
weight = 200
+++

Saving history of a table can be useful for auditability reasons. There are several
ways to do this. One is to do it in the application layer e.g. [django-simple-history](https://django-simple-history.readthedocs.io/en/latest/) is a way to do it in Django with "history" tables
that mirror the schema of the original table minus some things like unique constraints. 

Or, you do something in the database layer.

## "changelog" table

I came across [this thread](https://news.ycombinator.com/item?id=37610899) on HN. The linked piece: [All the ways to capture changes in Postgres](https://blog.sequin.io/all-the-ways-to-capture-changes-in-postgres/), has some suggestions when it comes to capturing
changes happening to a table. One of the suggested methods is to use "changelog" tables as explained. The goal in the linked essay is not to store the historical records once they've been created. It's assumed
that there's another process that's consuming these records. I'm interested in the former.


But this gist, [Building blocks for generic history-keeping in Postgres](https://gist.github.com/slotrans/353952c4f383596e6fe8777db5d098f0) has a much better example and implementation. It also suggests that
you should keep separate history table for each tables. I happen to agree with that.

## pgaudit

I'd never heard of [pgaudit](https://github.com/pgaudit/pgaudit/tree/master) before, but it 
seems like the most appropriate solution. It's a postgres extension, and apparently available 
even on RDS.

