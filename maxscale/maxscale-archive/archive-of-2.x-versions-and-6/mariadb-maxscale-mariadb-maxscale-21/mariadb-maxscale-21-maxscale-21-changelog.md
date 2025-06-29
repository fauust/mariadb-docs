# MaxScale 2.1 Changelog

### MariaDB MaxScale 2.1

* MariaDB MaxScale is licensed under MariaDB BSL 1.1.
* Hierarchical configuration files are now supported.
* Logging is now performed in a way compatible with logrotate(8).
* Persistent connections are reset upon resuse.
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

For more details, please refer to:[_MariaDB MaxScale 2.1.17 Release Notes_](https://github.com/mariadb-corporation/docs-server/blob/test/maxscale/mariadb-maxscale-mariadb-maxscale-21/Release-Notes/MaxScale-2.1.17-Release-Notes.md) [MariaDB MaxScale 2.1.16 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.15 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.14 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.13 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.12 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.11 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.10 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.9 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.8 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.7 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.6 Release Notes](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)[_MariaDB MaxScale 2.1.5 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.4 Release Notes](maxscale-21-release-notes/mariadb-maxscale-21-mariadb-maxscale-214-release-notes-2017-07-03.md)[_MariaDB MaxScale 2.1.3 Release Notes_](../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) [MariaDB MaxScale 2.1.2 Release Notes](maxscale-21-release-notes/mariadb-maxscale-21-mariadb-maxscale-212-release-notes-2017-04-03.md)[_MariaDB MaxScale 2.1.1 Release Notes_](maxscale-21-release-notes/mariadb-maxscale-21-mariadb-maxscale-211-release-notes-2017-03-14.md) [MariaDB MaxScale 2.1.0 Release Notes](maxscale-21-release-notes/mariadb-maxscale-21-mariadb-maxscale-210-release-notes-2017-02-16.md)

### MariaDB MaxScale 2.0

* MariaDB MaxScale is licensed under MariaDB BSL.
* SSL can be used in the communication between MariaDB MaxScale and the backend servers.
* The number of allowed connections can explicitly be throttled.
* MariaDB MaxScale can continue serving read request even if the master has gone down.
* The security of MaxAdmin has been improved; Unix domain sockets can be used in the\
  communication with MariaDB MaxScale and the Linux identity can be used for authorization.
* MariaDB MaxScale can in real time make binlog events available as raw AVRO or\
  as JSON objects (beta level functionality).

For more details, please refer to:[_MariaDB MaxScale 2.0.6 Release Notes_](https://github.com/mariadb-corporation/docs-server/blob/test/maxscale/mariadb-maxscale-mariadb-maxscale-21/Release-Notes/MaxScale-2.0.6-Release-Notes.md) [MariaDB MaxScale 2.0.5 Release Notes](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-205-release-notes-2017-03-10.md)[_MariaDB MaxScale 2.0.4 Release Notes_](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-204-release-notes-2017-02-01.md) [MariaDB MaxScale 2.0.3 Release Notes](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-203-release-notes.md)[_MariaDB MaxScale 2.0.2 Release Notes_](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-202-release-notes.md) [MariaDB MaxScale 2.0.1 Release Notes](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-201-release-notes.md)

* [MariaDB MaxScale 2.0.0 Release Notes](../mariadb-maxscale-mariadb-maxscale-20/maxscale-20-release-notes/mariadb-maxscale-20-mariadb-maxscale-200-release-notes.md)

### MariaDB MaxScale 1.4

* Authentication now allows table level resolution of grants. MaxScale service\
  users will now need SELECT privileges on `mysql.tables_priv` to be able to\
  authenticate users at the database and table level.
* Firewall filter allows whitelisting.
* Client side SSL works.

For more details, please refer to[_MariaDB MaxScale 1.4.3 Release Notes_](../mariadb-maxscale-14/maxscale-14-release-notes/mariadb-maxscale-143-release-notes.md) [MariaDB MaxScale 1.4.2 Release Notes](../mariadb-maxscale-14/maxscale-14-release-notes/mariadb-maxscale-142-release-notes.md)[_MariaDB MaxScale 1.4.1 Release Notes_](../mariadb-maxscale-14/maxscale-14-release-notes/mariadb-maxscale-141-release-notes.md) [MariaDB MaxScale 1.4.0 Release Notes](../mariadb-maxscale-14/maxscale-14-release-notes/mariadb-maxscale-140-beta-release-notes.md).

### MariaDB MaxScale 1.3

* Added support for persistent backend connections
* The binlog server is now an integral component of MariaDB MaxScale.
* The logging has been changed; instead of different log files there is one log file and different message priorities.

For more details, please refer to [MariaDB MaxScale 1.3 Release Notes](../mariadb-maxscale-14/maxscale-14-release-notes/mariadb-maxscale-13-release-notes.md)

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
