# Connection Routing with MariaDB MaxScale

The object of this tutorial is to have a system that has two ports\
available, one for write connections and another for read connection that\
are load balanced across all servers.

### Setting up MariaDB MaxScale

The first part of this tutorial is covered in [MariaDB MaxScale Tutorial](mariadb-maxscale-22-setting-up-mariadb-maxscale.md).\
Please read it and follow the instructions for setting up MariaDB MaxScale with\
the type of cluster you want to use.

Once you have MariaDB MaxScale installed and the database users created, we can\
create the configuration file for MariaDB MaxScale.

### Creating Your MariaDB MaxScale Configuration

MariaDB MaxScale reads its configuration from `/etc/maxscale.cnf`. A template\
configuration is provided with the MaxScale installation.

A global `[maxscale]` section is included in every MariaDB MaxScale\
configuration file; this is used to set the values of various global parameters,\
perhaps the most important of these is the number of threads that MariaDB\
MaxScale will use to handle client requests. To automatically configure the\
thread count, use the `threads=auto` parameter.

```
[maxscale]
threads=auto
```

### Configuring Servers

Read the [Configuring Servers](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) mini-tutorial to see how\
the servers are configured.

### Configuring the Monitor

The next step is the configuration of the monitor. This depends on the type of\
cluster you use with MaxScale.

For master-slave clusters read the[Configuring MariaDB Monitor](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)\
tutorial. If you are using a Galera cluster, read the[Configuring Galera Monitor](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/)\
tutorial instead.

### Configuring the Service

We want two different ports to which the client application can connect; one\
that will be directed to a server where writes can be sent and another that will\
load balance between all servers. To achieve this, we need to define two\
services in the configuration file.

Create the following two sections in your configuration file. The section\
names are the names of the services themselves and should be meaningful to\
the administrator. For this tutorial, we use the `Write-Service` and`Read-Service` names for our services.

```
[Write-Service]
type=service
router=readconnroute
router_options=master
servers=dbserv1, dbserv2, dbserv3
user=maxscale
password=maxscale_pw

[Read-Service]
type=service
router=readconnroute
router_options=slave
servers=dbserv1, dbserv2, dbserv3
user=maxscale
password=maxscale_pw
```

The router module for these two sections is identical, the `readconnroute`\
module.

The services must be provided with the list of servers where queries\
will be routed to. The server names given here are the names of server sections\
in the configuration file (to be defined later) and not the physical hostnames\
or addresses of the servers.

In order to instruct the router to which servers it should route we must add the`router_options` parameter to the service. This parameter tells what sort of\
servers the service will use. For the write service we use the _master_ type and\
for the read service we use the _slave_ type.

The final part of the service configuration is the `user` and `password`\
parameters that define the credentials that the service will use to populate the\
user authentication data. These users were created at the start of the[MaxScale Tutorial](mariadb-maxscale-22-setting-up-mariadb-maxscale.md).

**Note:** For increased security [encrypt your passwords in the configuration file](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/).

### Configuring the Listener

In order to allow network connections to the service, we must associate a network\
port with the service. This is done by creating a separate listener section in\
the configuration file. A service may have multiple listeners but for this\
tutorial we will only need one per service.

```
[Write-Listener]
type=listener
service=Write-Service
protocol=MariaDBClient
port=3306

[Read-Listener]
type=listener
service=Read-Service
protocol=MariaDBClient
port=3307
```

The `service` parameter tells to which service the listener connects to. For the`Write-Listener` we set it to `Write-Service` and for the `Read-Listener` we set\
it to `Read-Service`.

A listener must also define the protocol module it will use for the incoming\
network protocol (must be the `MariaDBClient` protocol for all database\
listeners) as well as the the network port to listen on.

Additionally, the `address` parameter may be given if the listener is required\
to bind to a particular network interface when the host machine has multiple\
network interfaces. The default behavior is to listen on all network interfaces\
(the IPv6 address `::`).

### Configuring the Administrative Interface

The MaxAdmin configuration is described in the[Configuring MaxAdmin](../../../archive-of-2x.xx-versions/mariadb-maxscale-21-06/) document.

### Starting MariaDB MaxScale

Upon completion of the configuration process MariaDB MaxScale is ready to be\
started for the first time. For newer systems that use systemd, use the _systemctl_ command.

```
sudo systemctl start maxscale
```

For older SysV systems, use the _service_ command.

```
sudo service maxscale start
```

If MaxScale fails to start, check the error log in`/var/log/maxscale/maxscale.log` to see if any errors are detected in the\
configuration file. The `maxadmin` command may be used to confirm that MariaDB\
MaxScale is running and the services, listeners and servers have been correctly\
configured.

```
% sudo maxadmin list services

Services.
--------------------------+-------------------+--------+----------------+-------------------
Service Name              | Router Module     | #Users | Total Sessions | Backend databases
--------------------------+-------------------+--------+----------------+-------------------
Write-Service             | readconnroute     |      1 |              1 | dbserv1, dbserv2, dbserv3
Read-Service              | readconnroute     |      1 |              1 | dbserv1, dbserv2, dbserv3
CLI                       | cli               |      2 |              3 |
--------------------------+-------------------+--------+----------------+-------------------

% sudo maxadmin list servers

Servers.
-------------------+-----------------+-------+-------------+--------------------
Server             | Address         | Port  | Connections | Status
-------------------+-----------------+-------+-------------+--------------------
dbserv1            | 192.168.2.1     |  3306 |           0 | Running, Slave
dbserv2            | 192.168.2.2     |  3306 |           0 | Running, Master
dbserv3            | 192.168.2.3     |  3306 |           0 | Running, Slave
-------------------+-----------------+-------+-------------+--------------------

% sudo maxadmin list listeners

Listeners.
---------------------+---------------------+--------------------+-----------------+-------+--------
Name                 | Service Name        | Protocol Module    | Address         | Port  | State
---------------------+---------------------+--------------------+-----------------+-------+--------
Write-Listener       | Write-Service       | MariaDBClient      | *               |  3306 | Running
Read-Listener        | Read-Service        | MariaDBClient      | *               |  3307 | Running
CLI-Listener         | CLI                 | maxscaled          | default         |     0 | Running
---------------------+---------------------+--------------------+-----------------+-------+--------
```

MariaDB MaxScale is now ready to start accepting client connections and routing\
them to the cluster. More options may be found in the[Configuration Guide](../maxscale-22-getting-started/mariadb-maxscale-22-mariadb-maxscale-configuration-usage-scenarios.md)\
and in the [readconnroute module documentation](../maxscale-22-routers/mariadb-maxscale-22-readconnroute.md).

More detail on the use of `maxadmin` can be found in the[MaxAdmin](../maxscale-22-reference/mariadb-maxscale-22-maxadmin-admin-interface.md) document.

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
