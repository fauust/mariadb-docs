# SHOW BINLOG EVENTS

## Syntax

```
SHOW BINLOG EVENTS
   [IN 'log_name'] [FROM pos] [LIMIT [offset,] row_count]
```

## Description

Shows the events in the [binary log](../../../../server-management/server-monitoring-logs/binary-log/). If you do not specify '`log_name`',\
the first binary log is displayed.

Requires the [BINLOG MONITOR](../../account-management-sql-statements/grant.md#binlog-monitor) privilege (>= [MariaDB 10.5.2](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1052-release-notes)) or the [REPLICATION SLAVE](../../account-management-sql-statements/grant.md#replication-slave) privilege (<= [MariaDB 10.5.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes)).

## Example

```
SHOW BINLOG EVENTS IN 'mysql_sandbox10019-bin.000002';
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
| Log_name                      | Pos | Event_type        | Server_id | End_log_pos | Info                                           |
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
| mysql_sandbox10019-bin.000002 |   4 | Format_desc       |         1 |         248 | Server ver: 10.0.19-MariaDB-log, Binlog ver: 4 |
| mysql_sandbox10019-bin.000002 | 248 | Gtid_list         |         1 |         273 | []                                             |
| mysql_sandbox10019-bin.000002 | 273 | Binlog_checkpoint |         1 |         325 | mysql_sandbox10019-bin.000002                  |
| mysql_sandbox10019-bin.000002 | 325 | Gtid              |         1 |         363 | GTID 0-1-1                                     |
| mysql_sandbox10019-bin.000002 | 363 | Query             |         1 |         446 | CREATE DATABASE blog                           |
| mysql_sandbox10019-bin.000002 | 446 | Gtid              |         1 |         484 | GTID 0-1-2                                     |
| mysql_sandbox10019-bin.000002 | 484 | Query             |         1 |         571 | use `blog`; CREATE TABLE bb (id INT)           |
+-------------------------------+-----+-------------------+-----------+-------------+------------------------------------------------+
```

<sub>_This page is licensed: GPLv2, originally from [fill\_help\_tables.sql](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)_</sub>

{% @marketo/form formId="4316" %}
