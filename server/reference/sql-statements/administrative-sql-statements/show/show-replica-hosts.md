# SHOW REPLICA HOSTS

## Syntax

```
SHOW { SLAVE | REPLICA } HOSTS
```

## Description

This command is run on the primary and displays a list of replicas that are currently registered with it. The output looks like this:

```
SHOW SLAVE HOSTS;
+------------+-----------+------+-----------+
| Server_id  | Host      | Port | Master_id |
+------------+-----------+------+-----------+
|  192168010 | iconnect2 | 3306 | 192168011 |
| 1921680101 | athena    | 3306 | 192168011 |
+------------+-----------+------+-----------+
```

* `Server_id`: The unique server ID of the replica server, as configured in the server's option file, or on the command line with [--server-id=value](../../../../ha-and-performance/standard-replication/replication-and-binary-log-system-variables.md).
* `Host`: The host name of the replica server, as configured in the server's option file, or on the command line with `--report-host=host_name` (note that this can differ from the machine name as configured in the operating system). Starting in [MariaDB 10.5.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes), if a replica doesn't configure `--report-host` explicitly, the value for the `Host` column is automatically extracted using the network connection's host name or IP address. Prior to 10.5, the Host value is left blank if a replica's `--report-host` parameter is not configured.
* `Port`: The port the replica server is listening on.
* `Master_id`: The unique server ID of the primary server that the replica server is replicating from.

Some MariaDB and MySQL versions report another variable, [rpl\_recovery\_rank](../../../../ha-and-performance/optimization-and-tuning/system-variables/server-system-variables.md#rpl_recovery_rank). This\
variable was never used, and was eventually removed in [MariaDB 10.1.2](https://github.com/mariadb-corporation/docs-server/blob/test/server/reference/sql-statements/administrative-sql-statements/show/broken-reference/README.md) .

Requires the [REPLICATION MASTER ADMIN](../../account-management-sql-statements/grant.md#replication-master-admin) privilege (>= [MariaDB 10.5.2](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1052-release-notes)) or the [REPLICATION SLAVE](../../account-management-sql-statements/grant.md#replication-slave) privilege (<= [MariaDB 10.5.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes)).

#### SHOW REPLICA HOSTS

**MariaDB starting with** [**10.5.1**](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes)

`SHOW REPLICA HOSTS` is an alias for `SHOW SLAVE HOSTS` from [MariaDB 10.5.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes).

## See Also

* [MariaDB replication](../../../../ha-and-performance/standard-replication/)
* [Replication threads](../../../../ha-and-performance/standard-replication/replication-threads.md)
* [SHOW PROCESSLIST](show-processlist.md). In `[SHOW PROCESSLIST](show-processlist.md)` output, replica threads are identified by `Binlog Dump`

<sub>_This page is licensed: GPLv2, originally from_</sub> [<sub>_fill\_help\_tables.sql_</sub>](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)

{% @marketo/form formId="4316" %}
