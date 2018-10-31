+++
draft = false
date = "2018-10-25T22:25:44-05:00"
title = "Fixing pglogical and growing postgres xlogs"

+++

Using [pglogical](https://www.2ndquadrant.com/en/resources/pglogical/) to stream to a standby postgres node is a simple and effective way to create an HA postgres cluster. We occasionally have issues where the xlog fills up the volume due to broken replication with pglogical.  If this happens here are some things check:

1. **Bad Tables or Columns**

    Check the Postgres logs of the hot standby. Usually replication will fail if a table or column structure is missing on the standy database. You might see failed sql queries on a table that doesn’t exist or a missing column/incorrect data type issues.

    To fix this -- add or alter the tables and/or columns to the slave. This should fix the transactions and the replication will be restored. You can either manually run the sql yourself or replicate the ddl changes via pglogical.

    See ddl replication commands in [pglogical docs](https://www.2ndquadrant.com/en/resources/pglogical/pglogical-docs/): `pglogical.replicate_ddl_command`

2. **Misconfigured pglogical setup**

    Sometimes when setting up streaming for the *first* time, its possible to misconfigure pglogical. This will cause your xlog to fill as pglogical waits for a viable node to connect and begin shipping the WAL logs.

    In the event this occurs, just blow away pglogical:

    `drop extension pglogical`

    Next, drop the replication slots:

    `select slot_name from pg_replication_slots where plugin = 'pglogical';`

    Then pass that `slot_name` to the following function:

    `select pg_drop_replication_slot('slot_name');`

    At this point, the xlog volume should return to normal.
