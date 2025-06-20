
# MONTH

## Syntax


```
MONTH(date)
```

## Description


Returns the month for `date` in the range 1 to 12 for January to
December, or 0 for dates such as '0000-00-00' or '2008-00-00' that
have a zero month part.


## Examples


```
SELECT MONTH('2019-01-03');
+---------------------+
| MONTH('2019-01-03') |
+---------------------+
|                   1 |
+---------------------+

SELECT MONTH('2019-00-03');
+---------------------+
| MONTH('2019-00-03') |
+---------------------+
|                   0 |
+---------------------+
```


<sub>_This page is licensed: GPLv2, originally from [fill\_help\_tables.sql](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)_</sub>


{% @marketo/form formId="4316" %}
