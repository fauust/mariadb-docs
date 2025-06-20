# DROP TRIGGER

## Syntax

```
DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
```

## Description

This statement drops a [trigger](../../../../server-usage/triggers-events/triggers/). The schema (database) name is optional. If the\
schema is omitted, the trigger is dropped from the default schema.\
Its use requires the `TRIGGER` privilege for the table associated with the trigger.

Use `IF EXISTS` to prevent an error from occurring for a\
trigger that does not exist. A `NOTE` is generated for a non-existent trigger\
when using `IF EXISTS`. See [SHOW WARNINGS](../../administrative-sql-statements/show/show-warnings.md).

**Note:** Triggers for a table are also dropped if you drop the table.

### Atomic DDL

**MariaDB starting with** [**10.6.1**](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-6-series/mariadb-1061-release-notes)

[MariaDB 10.6.1](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/mariadb-10-6-series/mariadb-1061-release-notes) supports [Atomic DDL](../atomic-ddl.md) and `DROP TRIGGER` is atomic.

## Examples

```
DROP TRIGGER test.example_trigger;
```

Using the IF EXISTS clause:

```
DROP TRIGGER IF EXISTS test.example_trigger;
Query OK, 0 rows affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+-------+------+------------------------+
| Level | Code | Message                |
+-------+------+------------------------+
| Note  | 1360 | Trigger does not exist |
+-------+------+------------------------+
```

## See Also

* [Trigger Overview](../../../../server-usage/triggers-events/triggers/trigger-overview.md)
* [CREATE TRIGGER](../../../../server-usage/triggers-events/triggers/create-trigger.md)
* [Information Schema TRIGGERS Table](../../administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-triggers-table.md)
* [SHOW TRIGGERS](../../administrative-sql-statements/show/show-triggers.md)
* [SHOW CREATE TRIGGER](../../administrative-sql-statements/show/show-create-trigger.md)
* [Trigger Limitations](../../../../server-usage/triggers-events/triggers/trigger-limitations.md)

<sub>_This page is licensed: GPLv2, originally from [fill\_help\_tables.sql](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)_</sub>

{% @marketo/form formId="4316" %}
