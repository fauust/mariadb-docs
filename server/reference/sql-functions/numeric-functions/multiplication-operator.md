# Multiplication Operator (\*)

## Syntax

```
*
```

## Description

Multiplication operator.

## Examples

```
SELECT 7*6;
+-----+
| 7*6 |
+-----+
|  42 |
+-----+

SELECT 1234567890*9876543210;
+-----------------------+
| 1234567890*9876543210 |
+-----------------------+
|  -6253480962446024716 |
+-----------------------+

SELECT 18014398509481984*18014398509481984.0;
+---------------------------------------+
| 18014398509481984*18014398509481984.0 |
+---------------------------------------+
|   324518553658426726783156020576256.0 |
+---------------------------------------+

SELECT 18014398509481984*18014398509481984;
+-------------------------------------+
| 18014398509481984*18014398509481984 |
+-------------------------------------+
|                                   0 |
+-------------------------------------+
```

## See Also

* [Type Conversion](../string-functions/type-conversion.md)
* [Addition Operator (+)](addition-operator.md)
* [Subtraction Operator (-)](../../sql-structure/operators/arithmetic-operators/subtraction-operator.md)
* [Division Operator (/)](division-operator.md)
* [Operator Precedence](../../sql-structure/operators/operator-precedence.md)

<sub>_This page is licensed: GPLv2, originally from [fill\_help\_tables.sql](https://github.com/MariaDB/server/blob/main/scripts/fill_help_tables.sql)_</sub>

{% @marketo/form formId="4316" %}
