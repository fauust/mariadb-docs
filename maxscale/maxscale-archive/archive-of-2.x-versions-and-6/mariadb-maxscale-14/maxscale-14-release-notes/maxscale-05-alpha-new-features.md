# MaxScale 0.5 Alpha New Features

0.5 Alpha

This document details the changes in version 0.5 since the release of the 0.4 alpha of the MaxScale product.

## New Features

### Read/Write Splitter Routing Module

In previous versions the read/write splitter routing module has had a number of limitations on it use, in the alpha release the router now removes the most important restrictions.

#### Session Commands

Session commands are those statements that make some change to the user’s login session that may cause different effects from subsequent statements executed. Since the read/write splitter executes statements on either a master server or a slave server, depending upon the statement to execute, it is important that these session modifications are executed on all connections to both slave and master servers. This is resolved in release 0.5 such that session modification commands are executed on all active connections and a single return is forward back to the client that made the request.

#### Transaction Support

Transaction support has been added into this version of the read/write splitter, there is one known outstanding limitation. If autocommit is enabled inside an active transaction it is not considered as commit in read/write splitter. Once a transaction has started all statements are routed to a master until the transaction is committed or rolled back.

### Authentication

A number of issues and shortcomings in the authentication performed by MaxScale have been resolved by this release.

#### Host Considered in Authentication

Previously MaxScale did not follow the same rules as MySQL when authenticating a login request, it would always use the wildcard password entries and would not check the incoming host was allowed to connect. MaxScale now checks the incoming IP address for a connection request and verifies this against the authentication data loaded from the backend servers. The same rules are applied when choosing the password entry to authenticate with. Note however that authentication from MaxScale to the backend database will fail if the MaxScale host is not allowed to login using the matching password for the user.

#### Stale Authentication Data

In previous releases of MaxScale the authentication data would be read at startup time only and would not be refreshed. Therefore if a user was added or modified in the backend server this will not be picked up by MaxScale and that user would be unable to connect via MaxScale. MaxScale now reloads user authentication data when a failure occurs and will refresh its internal tables if the data has changed in the backend. Please note that this reload process is rate limited to prevent incorrect logins to MaxScale being used for a denial of service attack on the backend servers.

#### Enable Use Of "root" User

Previously MaxScale would prevent the use of the root user to login to the backend servers via MaxScale. This may be enabled on a per service basis by adding an "enable\_root\_user" options in the service entry to enable it in the MaxScale configuration file. This allows the use of root to be controlled on a per service basis.

### Network Support

#### Unix Domain Sockets

MaxScale now supports Unix domain sockets for connecting to a local MaxScale server. The use of a Unix domain socket is controlled by adding a "socket" entry in the listener configuration entry for a service.

#### Network Interface Binding

MaxScale has added the ability to bind a listener for a service to a network address via an "address" entry in the configuration file.

## Server Version

The server version reported when connected to a database via MaxScale has now been altered. This now shows the MaxScale name and version together with the backend server name. An example of this can be seen below for the 0.5 release.

-bash-4.1$ mysql -h 127.0.0.1 -P 4006 -uxxxx -pxxxx\
Welcome to the MariaDB monitor. Commands end with ; or \g.\
Your MySQL connection id is 22320\
Server version: MaxScale 0.5.0 MariaDB Server

Copyright (c) 2000, 2012, Oracle, Monty Program Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

### MySQL \[(none)]> \ys

mysql Ver 15.1 Distrib 5.5.28a-MariaDB, for Linux (i686) using readline 5.1

...\
Server: MySQL\
Server version: MaxScale 0.5.0 MariaDB Server\
...

MySQL \[(none)]>

## Bug Fixes

A number of bug fixes have been applied between the 0.4 alpha and this alpha release. The table below lists the bugs that have been resolved. The details for each of these may be found in bugs.skysql.com.

|     |                                                                                                              |
| --- | ------------------------------------------------------------------------------------------------------------ |
| ID  | Summary                                                                                                      |
| 141 | No "delete user" command in debugcli                                                                         |
| 175 | Buffer leak in dcb\_read from Coverity run                                                                   |
| 178 | Uninitialised variables from Coverity run                                                                    |
| 179 | open with O\_CREAT in second argument needs 3 arguments                                                      |
| 363 | simple\_mutex "name" memory handling ...                                                                     |
| 126 | "reload config" in debug interface causes maxscale server to segfault                                        |
| 149 | It is possible to delete all maxscale users                                                                  |
| 218 | there is no way to understand what is going on if MAXSCALE\_HOME is incorrect                                |
| 137 | "show users" and "reload users" refer to very different things in debugcli                                   |
| 154 | readwritesplit does not use router\_options                                                                  |
| 160 | telnetd leaks memory                                                                                         |
| 169 | Galera monitor is actually never compiled ....                                                               |
| 172 | Several compile errors in galera\_mon.c                                                                      |
| 174 | Resource leak in server.c                                                                                    |
| 176 | Resource leak in gw\_utils.c                                                                                 |
| 362 | possible datadir\_cleanup() problems ...                                                                     |
| 124 | readconnroute does not validate router\_options                                                              |
| 153 | MaxScale fails when max connections are exceeded                                                             |
| 133 | MaxScale leaves lots of "data" directories sitting around $MAXSCALE\_HOME                                    |
| 166 | readwritesplit causes MaxScale segfault when starting up                                                     |
| 207 | Quitting telnet session causes maxscale to fail                                                              |
| 161 | Memory leak in load\_mysql\_users.                                                                           |
| 177 | Resource leak in secrets.c                                                                                   |
| 182 | On Startup logfiles are empty                                                                                |
| 135 | MaxScale unsafely handles empty passwords in getUsers                                                        |
| 145 | .secret file for encrypted passwords cyclicly searched                                                       |
| 171 | ifndef logic in build\_gateway.inc doesn't work, MARIADB\_SRC\_PATH from env not picked up                   |
| 173 | Resource leak in adminusers.c found by Coverity                                                              |
| 376 | Confusing Server Version                                                                                     |
| 370 | maxscale binary returns zero exit status on failures                                                         |
| 150 | telnetd listener should bind to 127.0.0.1 by default                                                         |
| 152 | listener configuration should support bind address                                                           |
| 373 | Documentation: it's not clear what privileges the maxscale user needs                                        |
| 128 | Maxscale prints debug information to terminal session when run in background                                 |
| 129 | MaxScale refuses to connect to server and reports nonsense error as a result                                 |
| 147 | Maxscale's hashtable fails to handle deletion of entries.                                                    |
| 148 | users data structure's stats have incorrect values.                                                          |
| 384 | MaxScale crashes if backend authentication fails                                                             |
| 210 | Bad timing in freeing readconnrouter's dcbs cause maxscale crash                                             |
| 403 | gwbuf\_free doesn't protect freeing shared buffer                                                            |
| 371 | If router module load fails, MaxScale goes to inifinite loop                                                 |
| 385 | MaxScale (DEBUG-version) dasserts if backend dcb is closed in the middle of client dcb performing close\_dcb |
| 386 | Starting MaxScale with -c pointing at existing file causes erroneous behavior                                |
| 209 | Error in backend hangs client connection                                                                     |
| 194 | maxscale crashes at start if module load fails                                                               |
| 369 | typo in "QUERY\_TYPE\_UNKNWON"                                                                               |
| 163 | MaxScale crashes with multiple threads                                                                       |
| 162 | threads parameter in configuration file is not effective                                                     |
| 400 | hastable\_get\_stats returns value of uninitialized value in 'nelems'                                        |
| 212 | Failing write causes maxscale to fail                                                                        |
| 222 | Double freeing mutex corrupts log                                                                            |
| 208 | current\_connection\_count is decreased multiple times per session, thus breaking load balancing logic       |
| 378 | Misspelling maxscale section name in config file crashes maxscale                                            |
| 399 | Every row in log starts with 0x0A00                                                                          |
| 205 | MaxScale crashes due SEGFAULT because return value of dcb\_read is not checked                               |
| 220 | Maxscale crash if socket listening fails in startup                                                          |
| 372 | Log manager hangs MaxScale if log string (mostly query length) exceeds block size                            |
| 397 | Free of uninitialised pointer if MAXSCALE\_HOME is not set                                                   |
| 402 | gw\_decode\_mysql\_server\_handshake asserts with mysql 5.1 backend                                          |
| 345 | MaxScale don't find backend servers if they are started after MaxScale                                       |
| 406 | Memory leak in dcb\_alloc()                                                                                  |
| 360 | MaxScale passwd option                                                                                       |
| 151 | Get parse\_sql failed on array INSERT                                                                        |
| 216 | Backend error handling doesn't update server's connection counter                                            |
| 127 | MaxScale should handle out-of-date backend auth data more gracefully                                         |
| 146 | "show dbusers" argument not documented                                                                       |
| 125 | readconnroute causes maxscale server crash if no slaves are available                                        |
| 375 | Tarball contains UID and maxscale base dir                                                                   |

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
