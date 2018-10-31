+++
draft = false
date = "2018-10-25T22:25:44-05:00"
title = "pglogical and growing postgres xlogs"

+++

Using pglogical to stream to a standby postgres node is very simple effective way to create an HA postgres cluster. We occasionally have issues where the xlog fills up the volume due to broken replication with pglogical.  If this happen here are some things check:

1.Check the Postgres logs of the hot standby. Usually replication will fail if a table or column structure is missing on the standy database. You might see failed sql queries on table that doesn’t exist or missing column/incorrect data type issues.

To fix -- add or alter the tables and/or columns to the slave and the replication will be restored. You can either manually run the sql yourself or replicate the ddl changes.

See ddl replication commands in [pglogical docs](https://www.2ndquadrant.com/en/resources/pglogical/pglogical-docs/): `pglogical.replicate_ddl_command`

2.Now if setting up streaming for the *first* time, you probably misconfigured pglogical and it will cause your xlog to fill as pglogical waits for a viable node to connect and begin shipping WAL logs.

In the event this occurs just blow away pglogical:

`drop extension pglogical`

Next, drop the replication slots:

`select slot_name from pg_replication_slots where plugin = 'pglogical';`

then pass that slot name to the following function:

`select pg_drop_replication_slot('slot_name');`

At this point, the xlog volume should return to normal.
