# BINLOG

## Syntax

```
BINLOG 'str'
```

## Description

`BINLOG` is an internal-use statement. It is generated by the[mariadb-binlog](../../../clients-and-utilities/logging-tools/mariadb-binlog/) program as the printable representation of certain events in [binary log](../../../server-management/server-monitoring-logs/binary-log/) files. The `'str'` value is a base 64-encoded\
string that the server decodes to determine the data change indicated by the\
corresponding event. This statement requires the [SUPER](../account-management-sql-statements/grant.md#super) privilege (<= [MariaDB 10.5.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1051-release-notes)) or the [BINLOG REPLAY](../account-management-sql-statements/grant.md#binlog-replay) privilege (>= [MariaDB 10.5.2](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-5-series/mariadb-1052-release-notes)).

## See Also

* [MariaDB replication](../../../ha-and-performance/standard-replication/)

<sub>_This page is licensed: GPLv2, originally from_</sub> [<sub>_fill\_help\_tables.sql_</sub>](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)

{% @marketo/form formId="4316" %}
