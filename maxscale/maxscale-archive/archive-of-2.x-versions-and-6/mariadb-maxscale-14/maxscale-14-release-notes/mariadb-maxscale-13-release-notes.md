# MariaDB MaxScale 1.3 Release Notes

This document describes the changes in release 1.3, when compared to\
release 1.2.1.

### 1.3.0

For any problems you encounter, please consider submitting a bug\
report at [Jira](https://jira.mariadb.org).

### New Features

#### Persistent Connections

MaxScale 1.3.0 introduces the concept of _Persistent Connections_. With\
that is meant that the connection from MaxScale to the backend server is\
not terminated even if the connection from the client to MaxScale is.\
If a client makes frequent short connections, there may be a benefit from\
using the _Persistent Connection_ feature as it may reduce the time it\
takes from establishing a connection from the client through MaxScale to\
the backend server.

**NOTE**: The persistent connections do not track session state. This means\
that changing the default database or modifying the session state will cause\
those changes to be active even for new connections. If you use queries with\
implicit databases or use connections with different client settings, you\
should take great care when using persistent connections.

Additional information is available in the following document:

* [Administration Tutorial](../maxscale-14-tutorials/maxscale-administration-tutorial.md#persistent-connections)

#### Binlog Server

There are new administrative commands: STOP SLAVE, START SLAVE, RESET SLAVE\
and CHANGE MASTER TO. The master server details are now provided by a\
master.ini file located in binlog directory and could be changed via\
CHANGE MASTER TO command issued via MySQL connection to MaxScale.

Before migrating to 1.3.0 it is necessary to put a writable master.ini file\
into binlog directory, containing these parameters:

```
[binlog_configuration]
master_host=127.0.0.1
master_port=3308
master_user=repl
master_password=somepass
filestem=repl-bin
```

Users may change parameters according to their configuration.

**Note**: the "servers" parameter is no longer required in the service\
definition.

Additional information is available in the following documents:[_Binlogrouter Tutorial_](../maxscale-14-tutorials/maxscale-as-a-replication-proxy.md) [Upgrading Binlogrouter to 1.3](../../mariadb-maxscale-mariadb-maxscale-21/maxscale-21-upgrading/6364.md)

* [Binlogrouter Documentation](../maxscale-14-routers/binlogrouter.md)

#### Logging Changes

Before 1.3, MaxScale logged data to four different log files; _error_,_message_, _trace_ and _debug_. Complementary and/or alternatively, MaxScale\
could also log to syslog, in which case messages intended for the error and\
message file were logged there. What files were enabled and written to was\
controlled by entries in the MaxScale configuration file.

This has now been changed so that MaxScale logs to a single\
file - _maxscale.log_ - and each logged entry is prepended with _error_,_warning_, _notice_, _info_ or _debug_, depending on the seriousness or\
priority of the message. The levels are the same as those of syslog.\
MaxScale is still capable of complementary or alternatively logging to syslog.

What used to be logged to the _message_ file is now logged as a _notice_\
message and what used to be written to the _trace_ file, is logged as a&#x6E;_&#x69;nfo_ message.

By default, _notice_, _warning_ and _error_ messages are logged, whil&#x65;_&#x69;nfo_ and _debug_ messages are not. Exactly what kind of messages are\
logged can be controlled via the MaxScale configuration file, but enabling\
and disabling different kinds of messages can also be performed at runtime\
from maxadmin.

Earlier, the _error_ and _message_ files were written to the filesystem,\
while the _trace_ and _debug_ files were written to shared memory. The\
one and only log file of MaxScale is now by default written to the filesystem.\
This will have performance implications if _info_ and _debug_ messages are\
enabled.

If you want to retain the possibility of turning on _info_ and _debug_\
messages, without it impacting the performance too much, the recommended\
approach is to add the following entries to the MaxScale configuration file:

```
[maxscale]
syslog=1
maxlog=0
log_to_shm=1
```

This will have the effect of MaxScale creating the _maxscale.log_ into\
shared memory, but not logging anything to it. However, all _notice_,_warning_ and _error_ messages will be logged to syslog.

Then, if there is a need to turn on _info_ messages that can be done via\
the maxadmin interface:

```
MaxScale> enable log-priority info
MaxScale> enable maxlog
```

Note that _info_ and _debug_ messages are never logged to syslog.

#### PCRE2 integration

MaxScale now uses the PCRE2 library for regular expressions. This has been\
integrated into the core configuration processing and most of the modules.\
The main module which uses this is the regexfilter which now fully supports\
the PCRE2 syntax with proper substitutions. For a closer look at how this\
differs from the POSIX regular expression syntax take a look at the[PCRE2 documentation](https://www.pcre.org/current/doc/html/pcre2syntax.html).

**Please note**, that the substitution string follows different rules than\
the traditional substitution strings. The usual way of referring to capture\
groups in the substitution string is with the backslash character followed\
by the capture group reference e.g. `\1` but the PCRE2 library uses the dollar\
character followed by the group reference. To quote the PCRE2 native API manual:

```
In the replacement string, which is interpreted as a UTF string in UTF mode, and is checked for UTF validity unless the PCRE2_NO_UTF_CHECK option is set, a dollar character is an escape character that can specify the insertion of characters from capturing groups in the pattern. The following forms are recognized:

  $$      insert a dollar character
  $<n>    insert the contents of group <n>
  ${<n>}  insert the contents of group <n>
```

#### Improved launchable scripts

The launchable scripts were modified to allow usage without wrapper scripts.\
The scripts are now executed as they are in the configuration files with certain\
keywords being replaced with the initiator, event and node list. For more\
details, please read the [Monitor Common](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) document.

### Bug fixes

[Here is a list of bugs fixed since the release of MaxScale 1.2.1.](https://jira.mariadb.org/browse/MXS-550?jql=project%20%3D%20MXS%20AND%20issuetype%20%3D%20Bug%20AND%20resolution%20in%20\(Fixed%2C%20Done\)%20AND%20fixVersion%20%3D%201.3.0)

* [MXS-559](https://jira.mariadb.org/browse/MXS-559): Crash due to debug assertion in readwritesplit
* [MXS-551](https://jira.mariadb.org/browse/MXS-551): Maxscale BETA 1.3.0 running as root
* [MXS-548](https://jira.mariadb.org/browse/MXS-548): Maxscale 1.2.1 crash on Ubuntu 4.04.3 x86\_64
* [MXS-508](https://jira.mariadb.org/browse/MXS-508): regex filter ignores username
* [MXS-505](https://jira.mariadb.org/browse/MXS-505): if Maxscale fails to start it goes to infinite "try-to-start and fail" loop
* [MXS-501](https://jira.mariadb.org/browse/MXS-501): USE hangs when Tee filter uses matching
* [MXS-500](https://jira.mariadb.org/browse/MXS-500): Tee filter hangs when statements aren't duplicated.
* [MXS-499](https://jira.mariadb.org/browse/MXS-499): Init script error on Debian Wheezy
* [MXS-494](https://jira.mariadb.org/browse/MXS-494): Weight calculation favors servers without connections
* [MXS-493](https://jira.mariadb.org/browse/MXS-493): SIGFPE when weightby parameter is 0 and using LEAST\_GLOBAL\_CONNECTIONS
* [MXS-492](https://jira.mariadb.org/browse/MXS-492): Segfault if server is missing weighting parameter
* [MXS-491](https://jira.mariadb.org/browse/MXS-491): MaxScale can time out systemd if startup of services takes too long
* [MXS-480](https://jira.mariadb.org/browse/MXS-480): Readwritesplit defaults cause connection pileup
* [MXS-479](https://jira.mariadb.org/browse/MXS-479): localtime must not be used in the multi-threaded program.
* [MXS-472](https://jira.mariadb.org/browse/MXS-472): Monitors update status in multiple steps
* [MXS-464](https://jira.mariadb.org/browse/MXS-464): Upgrade 1.2.0 to 1.2.1 blocking start of `maxscale` service
* [MXS-450](https://jira.mariadb.org/browse/MXS-450): Syslog default prefix is MaxScale not maxscale
* [MXS-447](https://jira.mariadb.org/browse/MXS-447): Monitors are started before they have been fully configured
* [MXS-436](https://jira.mariadb.org/browse/MXS-436): Invalid threads argument is ignored and MaxScale starts with one thread
* [MXS-431](https://jira.mariadb.org/browse/MXS-431): Backend authentication fails with schemarouter
* [MXS-429](https://jira.mariadb.org/browse/MXS-429): Binlog Router crashes due to segmentation fault with no meaningful error if no listener is configured
* [MXS-428](https://jira.mariadb.org/browse/MXS-428): Maxscale crashes at startup.
* [MXS-427](https://jira.mariadb.org/browse/MXS-427): Logging a large string causes a segmentation fault
* [MXS-417](https://jira.mariadb.org/browse/MXS-417): Single character wildcard doesn't work in MaxScale
* [MXS-416](https://jira.mariadb.org/browse/MXS-416): Orphan sessions appear after many network errors
* [MXS-415](https://jira.mariadb.org/browse/MXS-415): MaxScale 1.2.1 crashed with Signal 6 and 11
* [MXS-414](https://jira.mariadb.org/browse/MXS-414): Maxscale crashed every day!
* [MXS-413](https://jira.mariadb.org/browse/MXS-413): MaxAdmin hangs with show session
* [MXS-412](https://jira.mariadb.org/browse/MXS-412): show dbusers segmentation fault
* [MXS-409](https://jira.mariadb.org/browse/MXS-409): prepare should not hit all servers
* [MXS-408](https://jira.mariadb.org/browse/MXS-408): Connections to backend databases do not clear promptly
* [MXS-407](https://jira.mariadb.org/browse/MXS-407): Maxscale binlogrouter binlog names are unncessarily length-limited
* [MXS-405](https://jira.mariadb.org/browse/MXS-405): Maxscale bin router crash
* [MXS-403](https://jira.mariadb.org/browse/MXS-403): Monitor callback to DCBs evades thread control causing crashes
* [MXS-394](https://jira.mariadb.org/browse/MXS-394): Faults in regex\_replace function of regexfilter.c
* [MXS-392](https://jira.mariadb.org/browse/MXS-392): Update to "Rabbit MQ setup and MaxScale Integration" document
* [MXS-386](https://jira.mariadb.org/browse/MXS-386): max\_sescmd\_history should not close connections
* [MXS-385](https://jira.mariadb.org/browse/MXS-385): disable\_sescmd\_history can cause false data to be read.
* [MXS-379](https://jira.mariadb.org/browse/MXS-379): Incorrect handing of a GWBUF may cause SIGABRT.
* [MXS-376](https://jira.mariadb.org/browse/MXS-376): MaxScale terminates with SIGABRT.
* [MXS-373](https://jira.mariadb.org/browse/MXS-373): If config file is non-existent, maxscale crashes.
* [MXS-366](https://jira.mariadb.org/browse/MXS-366): Multi-source slave servers are not detected.
* [MXS-365](https://jira.mariadb.org/browse/MXS-365): Load data local infile connection abort when loading certain files
* [MXS-363](https://jira.mariadb.org/browse/MXS-363): rpm building seems to do something wrong with maxscale libraries
* [MXS-361](https://jira.mariadb.org/browse/MXS-361): crash on backend restart if persistent connections are in use
* [MXS-360](https://jira.mariadb.org/browse/MXS-360): Persistent connections: maxadmin reports 0 all the time even if connections are created
* [MXS-358](https://jira.mariadb.org/browse/MXS-358): Crash, Error in \`/usr/bin/maxscale': free(): invalid next size (fast)
* [MXS-352](https://jira.mariadb.org/browse/MXS-352): With no backend connection, services aren't started
* [MXS-351](https://jira.mariadb.org/browse/MXS-351): Router error handling can cause crash by leaving dangling DCB pointer
* [MXS-345](https://jira.mariadb.org/browse/MXS-345): maxscale.conf in /etc/init.d prevents puppet from starting maxscale
* [MXS-342](https://jira.mariadb.org/browse/MXS-342): When ini\_parse fails to parse config file, no log messages are printed.
* [MXS-333](https://jira.mariadb.org/browse/MXS-333): use\_sql\_variables\_in=master doesn't work
* [MXS-329](https://jira.mariadb.org/browse/MXS-329): The session pointer in a DCB can be null unexpectedly
* [MXS-323](https://jira.mariadb.org/browse/MXS-323): mysql\_client readwritesplit handleError seems using wrong dcb and cause wrong behavior
* [MXS-321](https://jira.mariadb.org/browse/MXS-321): Incorrect number of connections in maxadmin list view
* [MXS-310](https://jira.mariadb.org/browse/MXS-310): MaxScale 1.2 does not completely cleanly change to the maxscale user
* [MXS-297](https://jira.mariadb.org/browse/MXS-297): postinstall on debian copies wrong file in /etc/init.d
* [MXS-293](https://jira.mariadb.org/browse/MXS-293): Bug in init script, and maxscale --user=maxscale does run as root
* [MXS-291](https://jira.mariadb.org/browse/MXS-291): Random number generation has flaws
* [MXS-289](https://jira.mariadb.org/browse/MXS-289): Corrupted memory or empty value are in Master\_host field of SHOW SLAVE STATUS when master connection is broken
* [MXS-286](https://jira.mariadb.org/browse/MXS-286): Fix the content and format of MaxScale-HA-with-Corosync-Pacemaker document
* [MXS-283](https://jira.mariadb.org/browse/MXS-283): SSL connections leak memory
* [MXS-282](https://jira.mariadb.org/browse/MXS-282): Add example to "Routing Hints" document
* [MXS-281](https://jira.mariadb.org/browse/MXS-281): SELECT INTO OUTFILE query goes several times to one slave
* [MXS-280](https://jira.mariadb.org/browse/MXS-280): SELECT INTO OUTFILE query succeeds even if backed fails
* [MXS-276](https://jira.mariadb.org/browse/MXS-276): Memory leak of buffer in connection router readQuery
* [MXS-274](https://jira.mariadb.org/browse/MXS-274): Memory Leak
* [MXS-271](https://jira.mariadb.org/browse/MXS-271): Schemarouter and unknown databases
* [MXS-269](https://jira.mariadb.org/browse/MXS-269): Crash in MySQL backend protocol
* [MXS-260](https://jira.mariadb.org/browse/MXS-260): Multiple MaxScale processes
* [MXS-258](https://jira.mariadb.org/browse/MXS-258): ERR\_error\_string could overflow in future
* [MXS-254](https://jira.mariadb.org/browse/MXS-254): Failure to read configuration file results in no error log messages
* [MXS-251](https://jira.mariadb.org/browse/MXS-251): Non-thread safe strerror
* [MXS-220](https://jira.mariadb.org/browse/MXS-220): LAST\_INSERT\_ID() query is redirect to slave if function call is in where clause
* [MXS-210](https://jira.mariadb.org/browse/MXS-210): Check MaxScale user privileges
* [MXS-202](https://jira.mariadb.org/browse/MXS-202): User password not handled correctly
* [MXS-197](https://jira.mariadb.org/browse/MXS-197): Incorrect sequence of operations with DCB
* [MXS-196](https://jira.mariadb.org/browse/MXS-196): DCB state is changed prior to polling operation
* [MXS-195](https://jira.mariadb.org/browse/MXS-195): maxscaled.c ineffective DCB disposal
* [MXS-184](https://jira.mariadb.org/browse/MXS-184): init script issues in CentOS 7
* [MXS-183](https://jira.mariadb.org/browse/MXS-183): MaxScale crash after 'reload config'
* [MXS-111](https://jira.mariadb.org/browse/MXS-111): maxscale binlog events shown in show services seems to be double-counted for the master connection
* [MXS-54](https://jira.mariadb.org/browse/MXS-54): Write failed auth attempt to trace log
* [MXS-35](https://jira.mariadb.org/browse/MXS-35): bugzillaId-451: maxscale main() exit code is always 0 after it daemonizes
* [MXS-29](https://jira.mariadb.org/browse/MXS-29): bugzillaId-589: detect if MAXSCALE\_SCHEMA.HEARTBEAT table is not replicated
* [MXS-3](https://jira.mariadb.org/browse/MXS-3): Remove code for atomic\_add in skygw\_utils.cc

### Known Issues and Limitations

There are a number bugs and known limitations within this version of MaxScale,\
the most serious of this are listed below.

* MaxScale can not manage authentication that uses wildcard matching in hostnames in the mysql.user table of the backend database. The only wildcards that can be used are in IP address entries.
* When users have different passwords based on the host from which they connect MaxScale is unable to determine which password it should use to connect to the backend database. This results in failed connections and unusable usernames in MaxScale.
* The readconnroute module does not support sending of LONGBLOB data.
* Galera Cluster variables, such as @@wsrep\_node\_name, are not resolved by the embedded MariaDB parser.
* The Database Firewall filter does not support multi-statements. Using them will result in an error being sent to the client.
* The SSL support is known to be unstable.

### Packaging

RPM and Debian packages are provided for the Linux distributions supported\
by MariaDB Enterprise.

Packages can be downloaded [here](https://mariadb.com/resources/downloads).

### Source Code

The source code of MaxScale is tagged at GitHub with a tag, which is identical\
with the version of MaxScale. For instance, the tag of version 1.2.1 of MaxScale\
is 1.2.1. Further, _master_ always refers to the latest released non-beta version.

The source code is available [here](https://github.com/mariadb-corporation/MaxScale).

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
