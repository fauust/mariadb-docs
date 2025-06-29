# TIMESTAMPADD

## Syntax

```
TIMESTAMPADD(unit,interval,datetime_expr)
```

## Description

Adds the integer expression interval to the date or datetime\
expression datetime\_expr. The unit for interval is given by the unit\
argument, which should be one of the following values: MICROSECOND, SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR.

The unit value may be specified using one of keywords as shown, or\
with a prefix of SQL\_TSI\_. For example, DAY and SQL\_TSI\_DAY both are\
legal.

Before [MariaDB 5.5](https://app.gitbook.com/s/aEnK0ZXmUbJzqQrTjFyb/mariadb-community-server-release-notes/old-releases/release-notes-mariadb-5-5-series/changes-improvements-in-mariadb-5-5), FRAC\_SECOND was permitted as a synonym for MICROSECOND.

## Examples

```
SELECT TIMESTAMPADD(MINUTE,1,'2003-01-02');
+-------------------------------------+
| TIMESTAMPADD(MINUTE,1,'2003-01-02') |
+-------------------------------------+
| 2003-01-02 00:01:00                 |
+-------------------------------------+

SELECT TIMESTAMPADD(WEEK,1,'2003-01-02');
+-----------------------------------+
| TIMESTAMPADD(WEEK,1,'2003-01-02') |
+-----------------------------------+
| 2003-01-09                        |
+-----------------------------------+
```

<sub>_This page is licensed: GPLv2, originally from [fill\_help\_tables.sql](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)_</sub>

{% @marketo/form formId="4316" %}
