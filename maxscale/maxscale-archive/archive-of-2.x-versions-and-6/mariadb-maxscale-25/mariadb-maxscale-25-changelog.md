# MaxScale 2.5 Changelog

### MariaDB MaxScale 2.5

* MaxAdmin has been removed.
* MaxGUI, a new browser based tool for configuring and managing\
  MaxScale is introduced.
* MaxInfo-router and the related httpd-protocol have been removed.
* Server weights have been removed.
* Services can now directly route to other services with the help of the[target](maxscale-25-getting-started/mariadb-maxscale-25-mariadb-maxscale-configuration-guide.md#target) parameter.
* Server parameters protocol and authenticator have been deprecated. Any\
  definitions are ignored.
* Listeners support multiple authenticators.
* The replication lag of a slave server must now be less than[max\_slave\_replication\_lag](maxscale-25-routers/mariadb-maxscale-25-readwritesplit.md#max_slave_replication_lag)\
  whereas in older versions the replication lag had to be less than or\
  equal to the configured limit.
* The global settings auth\_read\_timeout and auth\_write\_timeout have been\
  deprecated. Any definitions are ignored.
* The Columnstore monitor is now capable of monitoring Columnstore 1.5 in\
  addition to 1.0 and 1.2.
* MariaDB-Monitor supports cooperative monitoring. See[cooperative monitoring](maxscale-25-monitors/mariadb-maxscale-25-mariadb-monitor.md#cooperative-monitoring)\
  for more information.
* The MaxScale cache can now be shared between two MaxScale instances,\
  in which case either memcached or Redis can be used as cache storage.\
  Further, the cache can now also perform table level invalidations\
  and be specific to a particular user.
* A completely new binlog router implemenation.
* New routers, [mirror](maxscale-25-routers/mariadb-maxscale-25-mirror.md) and [kafkacdc](maxscale-25-routers/mariadb-maxscale-25-kafkacdc.md).
* Service-to-service routing is now possible with the `targets` parameter.
* TLS CRL and peer host verification support.
* Multiple modes of operation for `causal_reads`.

For more details, please refer to:

* [MariaDB MaxScale 2.5.29 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.5.29-Release-Notes)
* [MariaDB MaxScale 2.5.28 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2528-release-notes-2023-08-21.md)
* [MariaDB MaxScale 2.5.27 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2527-release-notes-2023-07-27.md)
* [MariaDB MaxScale 2.5.26 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2526-release-notes-2023-05-23.md)
* [MariaDB MaxScale 2.5.25 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2525-release-notes-2023-03-30.md)
* [MariaDB MaxScale 2.5.24 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2524-release-notes-2023-01-04.md)
* [MariaDB MaxScale 2.5.23 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2523-release-notes.md)
* [MariaDB MaxScale 2.5.22 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2522-release-notes-2022-10-11.md)
* [MariaDB MaxScale 2.5.21 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2521-release-notes-2022-07-08.md)
* [MariaDB MaxScale 2.5.20 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2520-release-notes-2022-05-10.md)
* [MariaDB MaxScale 2.5.19 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2519-release-notes-2022-02-11.md)
* [MariaDB MaxScale 2.5.18 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2518-release-notes-2022-01-12.md)
* [MariaDB MaxScale 2.5.17 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2517-release-notes.md)
* [MariaDB MaxScale 2.5.16 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2516-release-notes-2021-10-12.md)
* [MariaDB MaxScale 2.5.15 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2515-release-notes-2021-08-18.md)
* [MariaDB MaxScale 2.5.14 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2514-release-notes-2021-07-21.md)
* [MariaDB MaxScale 2.5.13 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2513-release-notes-2021-06-04.md)
* [MariaDB MaxScale 2.5.12 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2512-release-notes-2021-05-26.md)
* [MariaDB MaxScale 2.5.11 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2511-release-notes-2021-05-04.md)
* [MariaDB MaxScale 2.5.10 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-2510-release-notes-2021-03-25.md)
* [MariaDB MaxScale 2.5.9 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-259-release-notes-2021-03-10.md)
* [MariaDB MaxScale 2.5.8 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-258-release-notes-2021-02-18.md)
* [MariaDB MaxScale 2.5.7 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-257-release-notes-2021-01-27.md)
* [MariaDB MaxScale 2.5.6 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-256-release-notes-2020-12-15.md)
* [MariaDB MaxScale 2.5.5 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-255-release-notes-2020-10-21.md)
* [MariaDB MaxScale 2.5.4 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-254-release-notes-2020-09-29.md)
* [MariaDB MaxScale 2.5.3 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-253-release-notes-2020-08-31.md)
* [MariaDB MaxScale 2.5.2 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-252-release-notes-2020-08-14.md)
* [MariaDB MaxScale 2.5.1 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-251-release-notes-2020-07-16.md)
* [MariaDB MaxScale 2.5.0 Release Notes](maxscale-25-release-notes/mariadb-maxscale-25-mariadb-maxscale-250-release-notes-2020-06-18.md)

### MariaDB MaxScale 2.4

* A Clustrix specific monitor has been added.
* A new router, Smart Router, capable of routing a query to different\
  backends depending on the characteristics of the query has been added.
* Transaction replaying is now performed also in conjunction with server\
  initiated transaction rollbacks.
* Names starting with `@@` are reserved for use by MaxScale.
* Names can no longer contain whitespace.
* Servers can now be drained.
* The servers of a service can now be defined using a monitor.
* Durations can now be specified as hours, minutes, seconds or milliseconds.
* MaxCtrl commands `list sessions`, `show sessions` and `show session <id>`\
  support reverse DNS lookup of client addresses. The conversion is activated\
  by adding the `--rdns`-option to the command.
* The following MariaDB-Monitor settings have been removed and cause a startup error\
  if defined: `mysql51_replication`, `multimaster` and `allow_cluster_recovery`. The\
  setting `detect_replication_lag` is deprecated and ignored.
* `enforce_simple_topology`-setting added to MariaDB-Monitor.
* The mqfilter has been deprecated.

For more details, please refer to:

* [MariaDB MaxScale 2.4.19 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.19-Release-Notes)
* [MariaDB MaxScale 2.4.18 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.18-Release-Notes)
* [MariaDB MaxScale 2.4.17 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.17-Release-Notes)
* [MariaDB MaxScale 2.4.16 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.16-Release-Notes)
* [MariaDB MaxScale 2.4.15 Release Notes](https://mariadb.com/kb/en/mariadb-maxscale-25-mariadb-maxscale-2415-release-notes-2021-01-21/)
* [MariaDB MaxScale 2.4.14 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.14-Release-Notes)
* [MariaDB MaxScale 2.4.13 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.13-Release-Notes)
* [MariaDB MaxScale 2.4.12 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.12-Release-Notes)
* [MariaDB MaxScale 2.4.11 Release Notes](https://mariadb.com/kb/en/mariadb-maxscale-2411-release-notes-2020-07-13/)
* [MariaDB MaxScale 2.4.10 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.10-Release-Notes)
* [MariaDB MaxScale 2.4.9 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.9-Release-Notes)
* [MariaDB MaxScale 2.4.8 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.8-Release-Notes)
* [MariaDB MaxScale 2.4.7 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.7-Release-Notes)
* [MariaDB MaxScale 2.4.6 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.6-Release-Notes)
* [MariaDB MaxScale 2.4.5 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.5-Release-Notes)
* [MariaDB MaxScale 2.4.4 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.4-Release-Notes)
* [MariaDB MaxScale 2.4.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.3-Release-Notes)
* [MariaDB MaxScale 2.4.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.2-Release-Notes)
* [MariaDB MaxScale 2.4.1 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.1-Release-Notes)
* [MariaDB MaxScale 2.4.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.4.0-Release-Notes)

### MariaDB MaxScale 2.3

* Runtime Configuration of the Cache
* User Specified Syslog Facility and Level for Authentication Errors
* `config reload` removed from MaxAdmin (was deprecated in 2.2)
* MariaDBMonitor features added, modified and removed
* A Comment filter has been added.
* Services and filters can be created at runtime via the REST API
* Runtime router reconfiguration is now possible
* New Throttle filter that replaces and extends on the limit\_queries functionality
* MaxCtrl
* The `create monitor` command now accepts a list of key-value parameters
* The new `drain server` drains the server of connections
* A new interactive input mode was added
* Readwritesplit
* Automatic transaction replay allows transactions to be migrated between servers
* Master connections can now be re-opened
* Writes with autocommit enabled can be automatically retried
* Consistent reads on slaves via MASTER\_GTID\_WAIT
* Transaction load balancing for normal transactions
* Support for runtime router reconfiguration
* A new load balancing method: ADAPTIVE\_ROUTING
* Experimental resultset concatenation router, `cat`
* The schema router is now capable of table family sharding.
* The binlog router can now automatically switch to secondary masters\
  when replicating from a Galera cluster in case the primary master\
  goes down.
* MaxScale now has a systemd compatible watchdog.
* Server setting `authenticator_options` is no longer used and any value is\
  ignored.

For more details, please refer to:

* [MariaDB MaxScale 2.3.20 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.20-Release-Notes)
* [MariaDB MaxScale 2.3.19 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.19-Release-Notes)
* [MariaDB MaxScale 2.3.18 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.18-Release-Notes)
* [MariaDB MaxScale 2.3.17 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.17-Release-Notes)
* [MariaDB MaxScale 2.3.16 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.16-Release-Notes)
* [MariaDB MaxScale 2.3.15 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.15-Release-Notes)
* [MariaDB MaxScale 2.3.14 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.14-Release-Notes)
* [MariaDB MaxScale 2.3.13 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.13-Release-Notes)
* [MariaDB MaxScale 2.3.12 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.12-Release-Notes)
* [MariaDB MaxScale 2.3.11 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.11-Release-Notes)
* [MariaDB MaxScale 2.3.10 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.10-Release-Notes)
* [MariaDB MaxScale 2.3.9 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.9-Release-Notes)
* [MariaDB MaxScale 2.3.8 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.8-Release-Notes)
* [MariaDB MaxScale 2.3.7 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.7-Release-Notes)
* [MariaDB MaxScale 2.3.6 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.6-Release-Notes)
* [MariaDB MaxScale 2.3.5 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.5-Release-Notes)
* [MariaDB MaxScale 2.3.4 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.4-Release-Notes)
* [MariaDB MaxScale 2.3.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.3-Release-Notes)
* [MariaDB MaxScale 2.3.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.2-Release-Notes)
* [MariaDB MaxScale 2.3.1 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.1-Release-Notes)
* [MariaDB MaxScale 2.3.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.3.0-Release-Notes)

### MariaDB MaxScale 2.2

* Limited support from Pluggable Authentication Modules (PAM).
* Proxy protocol support for backend connections.
* REST-API for obtaining information about and for manipulating the\
  resources of MaxScale.
* MaxCtrl, a new command line client for administering MaxScale\
  implemented in terms of the REST-API.
* Firewall can now prevent the use of functions in conjunction with\
  certain columns.
* Parser of MaxScale extended to support window functions and CTEs.
* Parser of MaxScale extended to support PL/SQL compatibility features\
  of upcoming 10.3 release.
* Prepared statements are now parsed and the execution of read only\
  ones will be routed to slaves.
* Server states are persisted, so in case of crash and restart MaxScale\
  has the correct server state quicker.
* Monitor scripts are executed synchronously, so they can safely perform\
  actions that change the server states.
* The Masking filter can now both obfuscate and partially mask columns.
* Binlog router supports MariaDB 10 GTID at both ends.
* KILL CONNECTION can now be used through MaxScale.
* Environment variables can now be used in the MaxScale configuration file.
* By default, MaxScale can no longer be run as root.
* The MySQL Monitor is now capable of performing failover and switchover of\
  the master. There is also limited capability for rejoining nodes.

For more details, please refer to:

* [MariaDB MaxScale 2.2.21 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.21-Release-Notes)
* [MariaDB MaxScale 2.2.20 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.20-Release-Notes)
* [MariaDB MaxScale 2.2.19 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.19-Release-Notes)
* [MariaDB MaxScale 2.2.18 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.18-Release-Notes)
* [MariaDB MaxScale 2.2.17 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.17-Release-Notes)
* [MariaDB MaxScale 2.2.16 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.16-Release-Notes)
* [MariaDB MaxScale 2.2.15 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.15-Release-Notes)
* [MariaDB MaxScale 2.2.14 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.14-Release-Notes)
* [MariaDB MaxScale 2.2.13 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.13-Release-Notes)
* [MariaDB MaxScale 2.2.12 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.12-Release-Notes)
* [MariaDB MaxScale 2.2.11 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.11-Release-Notes)
* [MariaDB MaxScale 2.2.10 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.10-Release-Notes)
* [MariaDB MaxScale 2.2.9 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.9-Release-Notes)
* [MariaDB MaxScale 2.2.8 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.8-Release-Notes)
* [MariaDB MaxScale 2.2.7 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.7-Release-Notes)
* [MariaDB MaxScale 2.2.6 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.6-Release-Notes)
* [MariaDB MaxScale 2.2.5 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.5-Release-Notes)
* [MariaDB MaxScale 2.2.4 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.4-Release-Notes)
* [MariaDB MaxScale 2.2.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.3-Release-Notes)
* [MariaDB MaxScale 2.2.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.2-Release-Notes)
* [MariaDB MaxScale 2.2.1 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.1-Release-Notes)
* [MariaDB MaxScale 2.2.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.2.0-Release-Notes)

### MariaDB MaxScale 2.1

* MariaDB MaxScale is licensed under MariaDB BSL 1.1.
* Hierarchical configuration files are now supported.
* Logging is now performed in a way compatible with logrotate(8).
* Persistent connections are reset upon reuse.
* Galera monitor now consistently chooses the same node as master.
* Galera Monitor can set the preferred donor nodes list.
* The configuration can now be altered dynamically and the changes are persisted.
* There is now a monitor for Amazon Aurora clusters.
* MySQL Monitor now has a multi-master mode.
* MySQL Monitor now has a failover mode.
* Named Server Filter now supports wildcards for source option.
* Binlog Server can now be configured to encrypt binlog files.
* New filters, cache, ccrfilter, insertstream, masking, and maxrows are introduced.
* GSSAPI based authentication can be used
* Prepared statements are now in the database firewall filtered exactly like non-prepared\
  statements.
* The firewall filter can now filter based on function usage.
* MaxScale now supports IPv6

For more details, please refer to:

* [MariaDB MaxScale 2.1.17 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.17-Release-Notes)
* [MariaDB MaxScale 2.1.16 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.16-Release-Notes)
* [MariaDB MaxScale 2.1.15 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.15-Release-Notes)
* [MariaDB MaxScale 2.1.14 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.14-Release-Notes)
* [MariaDB MaxScale 2.1.13 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.13-Release-Notes)
* [MariaDB MaxScale 2.1.12 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.12-Release-Notes)
* [MariaDB MaxScale 2.1.11 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.11-Release-Notes)
* [MariaDB MaxScale 2.1.10 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.10-Release-Notes)
* [MariaDB MaxScale 2.1.9 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.9-Release-Notes)
* [MariaDB MaxScale 2.1.8 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.8-Release-Notes)
* [MariaDB MaxScale 2.1.7 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.7-Release-Notes)
* [MariaDB MaxScale 2.1.6 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.6-Release-Notes)
* [MariaDB MaxScale 2.1.5 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.5-Release-Notes)
* [MariaDB MaxScale 2.1.4 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.4-Release-Notes)
* [MariaDB MaxScale 2.1.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.3-Release-Notes)
* [MariaDB MaxScale 2.1.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.2-Release-Notes)
* [MariaDB MaxScale 2.1.1 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.1-Release-Notes)
* [MariaDB MaxScale 2.1.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.1.0-Release-Notes)

### MariaDB MaxScale 2.0

* MariaDB MaxScale is licensed under MariaDB BSL.
* SSL can be used in the communication between MariaDB MaxScale and the backend servers.
* The number of allowed connections can explicitly be throttled.
* MariaDB MaxScale can continue serving read request even if the master has gone down.
* The security of MaxAdmin has been improved; Unix domain sockets can be used in the\
  communication with MariaDB MaxScale and the Linux identity can be used for authorization.
* MariaDB MaxScale can in real time make binlog events available as raw AVRO or\
  as JSON objects (beta level functionality).

For more details, please refer to:

* [MariaDB MaxScale 2.0.6 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.6-Release-Notes)
* [MariaDB MaxScale 2.0.5 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.5-Release-Notes)
* [MariaDB MaxScale 2.0.4 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.4-Release-Notes)
* [MariaDB MaxScale 2.0.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.3-Release-Notes)
* [MariaDB MaxScale 2.0.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.2-Release-Notes)
* [MariaDB MaxScale 2.0.1 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.1-Release-Notes)
* [MariaDB MaxScale 2.0.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-2.0.0-Release-Notes)

### MariaDB MaxScale 1.4

* Authentication now allows table level resolution of grants. MaxScale service\
  users will now need SELECT privileges on `mysql.tables_priv` to be able to\
  authenticate users at the database and table level.
* Firewall filter allows whitelisting.
* Client side SSL works.

For more details, please refer to[_MariaDB MaxScale 1.4.3 Release Notes_](https://mariadb.com/kb/en/Release-Notes/MaxScale-1.4.3-Release-Notes) [MariaDB MaxScale 1.4.2 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-1.4.2-Release-Notes)[_MariaDB MaxScale 1.4.1 Release Notes_](https://mariadb.com/kb/en/Release-Notes/MaxScale-1.4.1-Release-Notes) [MariaDB MaxScale 1.4.0 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-1.4.0-Release-Notes).

### MariaDB MaxScale 1.3

* Added support for persistent backend connections
* The binlog server is now an integral component of MariaDB MaxScale.
* The logging has been changed; instead of different log files there is one log file and different message priorities.

For more details, please refer to [MariaDB MaxScale 1.3 Release Notes](https://mariadb.com/kb/en/Release-Notes/MaxScale-1.3.0-Release-Notes)

### MariaDB MaxScale 1.2

* Logfiles have been renamed. The log names are now named error.log, messages.log, trace.log and debug.log.

### MariaDB MaxScale 1.1.1

* Schemarouter now also allows for an upper limit to session commands.
* Schemarouter correctly handles SHOW DATABASES responses that span multiple buffers.
* Readwritesplit and Schemarouter now allow disabling of the session command history.

### MariaDB MaxScale 1.1

**NOTE:** MariaDB MaxScale default installation directory has changed to `/usr/local/mariadb-maxscale` and the default password for MaxAdmin is now ´mariadb´.

* New modules added
  * Binlog router
  * Firewall filter
  * Multi-Master monitor
  * RabbitMQ logging filter
  * Schema Sharding router
* Added option to use high precision timestamps in logging.
* Readwritesplit router now returns the master server's response.
* New readwritesplit router option added. It is now possible to control the amount of memory readwritesplit sessions will consume by limiting the amount of session modifying statements they can execute.
* Minimum required CMake version is now 2.8.12 for package building.
* Session idle timeout added for services. More details can be found in the configuration guide.
* Monitor API is updated to 2.0.0. Monitors with earlier versions of the API no longer work with this version of MariaDB MaxScale.
* MariaDB MaxScale now requires libcurl and libcurl development headers.
* Nagios plugins added.
* Notification service added.
* Readconnrouter has a new "running" router\_option. This allows it to use any running server as a valid backend server.
* Database names can be stripped of escape characters with the `strip_db_esc` service parameter.

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
