---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Execution Plan (CSEP)

## Overview

The ColumnStore storage engine uses a ColumnStore Execution Plan (CSEP) to represent a query plan internally.

When the select handler receives the `SELECT_LEX` object, it transforms it into a CSEP as part of the query planning and optimization process. For additional information, see "[MariaDB Enterprise ColumnStore Query Evaluation](../../../architecture/mariadb-enterprise-columnstore-query-evaluation.md)."

## Viewing the CSEP

The CSEP for a given query can be viewed by performing the following:

1. Calling the `calSetTrace(1)` function:

```
SELECT calSetTrace(1);
```

2. Executing the query:

```
SELECT column1, column2
FROM columnstore_tab
WHERE column1 > '2020-04-01'
AND column1 < '2020-11-01';
```

3. Calling the `calGetTrace()` function:

```
SELECT calGetTrace();
```

<sub>_This page is: Copyright © 2025 MariaDB. All rights reserved._</sub>

{% @marketo/form formId="4316" %}
