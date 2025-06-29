# About MariaDB MaxScale 2.0

## About MariaDB MaxScale 2.0

## About MariaDB MaxScale

**MariaDB MaxScale** is a database proxy that allows the forwarding of\
database statements to one or more database servers.

The forwarding is performed using rules that can be based on a the semantic\
understanding of the database statements and on the roles of the various\
servers within the backend cluster of databases.

MariaDB MaxScale is designed to provide, transparently to applications, load\
balancing and high availability functionality. In addition, it provides\
a highly scalable and flexible architecture, with plugin components to\
support different protocols and routing approaches.

MariaDB MaxScale makes extensive use of the asynchronous I/O capabilities of the\
Linux operating system, combined with a fixed number of worker threads.\
The epoll system is used to provide the event driven framework for the\
input and output via sockets. Similar features in Windows® could\
be used in future development of MariaDB MaxScale.

Many of the services provided by MariaDB MaxScale are implemented as external\
shared object modules, which are loaded at runtime. These modules\
support a fixed interface, communicating the entry points via a structure\
consisting of a set of function pointers. This structure is called the\
"module object". Additional modules can be created to work with MariaDB MaxScale.

One group of modules provides support for protocols, both for clients\
that communicate with MariaDB MaxScale and for backend servers. The code that\
routes the queries to the backend servers is also loaded as external\
shared objects and they are referred to as routing modules. Another\
group of modules work on data as it passes through MariaDB MaxScale, and they\
are known as filters.

A Google Group exists for MariaDB MaxScale that can be used to discuss ideas,\
issues and communicate with the MariaDB MaxScale community:\
Send email to [maxscale@googlegroups.com](mailto:maxscale@googlegroups.com)\
or use the [forum](https://groups.google.com/forum/#!forum/maxscale) interface

Bugs can be reported in the MariaDB Jira[jira.mariadb.org](https://jira.mariadb.org)

### Installing MariaDB MaxScale

Information about installing MariaDB MaxScale, either from a repository or by\
building from source code, is included in the[MariaDB MariaDB MaxScale Installation Guide](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/).

The same guide also provides basic information on running MariaDB MaxScale.\
More detailed information about configuring MariaDB MaxScale is given in the[Configuration Guide](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/).

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
