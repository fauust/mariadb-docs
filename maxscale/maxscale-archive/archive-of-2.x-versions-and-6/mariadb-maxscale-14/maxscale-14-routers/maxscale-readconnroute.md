# MaxScale Readconnroute

This document provides an overview of the **readconnroute** router module and its intended use case scenarios. It also displays all router configuration parameters with their descriptions.

### Overview

The readconnroute router provides simple and lightweight load balancing across a set of servers. The router can also be configured to balance connections based on a weighting parameter defined in the server's section.

### Configuration

Readconnroute router-specific settings are specified in the configuration file of MaxScale in its specific section. The section can be freely named but the name is used later as a reference from listener section.

For more details about the standard service parameters, refer to the [Configuration Guide](../maxscale-14-getting-started/maxscale-configuration-usage-scenarios.md).

### Router Options

**`router_options`** can contain a list of valid server roles. These roles are used as the valid types of servers the router will form connections to when new sessions are created.

```
router_options=slave
```

Here is a list of all possible values for the `router_options`.

| Role    | Description                                                                                                                                                                                                    |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Role    | Description                                                                                                                                                                                                    |
| master  | A server assigned as a master by one of MaxScale monitors. Depending on the monitor implementation, this could be a master server of a Master-Slave replication cluster or a Write-Master of a Galera cluster. |
| slave   | A server assigned as a slave of a master.                                                                                                                                                                      |
| synced  | A Galera cluster node which is in a synced state with the cluster.                                                                                                                                             |
| ndb     | A MySQL Replication Cluster node                                                                                                                                                                               |
| running | A server that is up and running. All servers that MaxScale can connect to are labeled as running.                                                                                                              |

If no `router_options` parameter is configured in the service definition, the router will use the default value of `running`. This means that it will load balance connections across all running servers defined in the `servers` parameter of the service.

### Limitations

For a list of readconnroute limitations, please read the [Limitations](../about-maxscale-14/limitations-and-known-issues-within-maxscale.md) document.

### Examples

The most common use for the readconnroute is to provide either a read or write port for an application. This provides a more lightweight routing solution than the more complex readwritesplit router but requires the application to be able to use distinct write and read ports.

To configure a read-only service that tolerates master failures, we first need to add a new section in to the configuration file.

```
[Read Service]
type=service
router=readconnroute
servers=slave1,slave2,slave3
router_options=slave
```

Here the `router_options` designates slaves as the only valid server type. With this configuration, the queries are load balanced across the slave servers.

For more complex examples of the readconnroute router, take a look at the examples in the [Tutorials](https://mariadb.com/kb/Tutorials) folder.

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
