# Setting up MaxScale

This document is designed as a quick introduction to setting up MaxScale in an environment in which you have either a MySQL Master-Slave replication cluster with one master and multiple slave servers or a multi-node Galera cluster. The process of setting and configuring MaxScale will be covered within this document.

The installation and configuration of the MySQL Replication or the Galera cluster will not be covered nor will any discussion of installation management tools to handle automated or semi-automated failover of the replication cluster. The [Setting Up Replication](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/ha-and-performance/standard-replication/setting-up-replication) article on the MariaDB knowledgebase can help you get started with replication clusters and the [Getting Started With Mariadb Galera Cluster](https://app.gitbook.com/s/3VYeeVGUV4AMqrA3zwy7/galera-management/getting-started-with-mariadb-galera-cluster) article will help you set up a Galera cluster.

This tutorial will assume the user is running from one of the binary distributions available and has installed this in the default location. Building from source code in GitHub is covered in the [Building from Source](../maxscale-14-getting-started/building-maxscale-from-source-code.md) document.

### Process

The steps involved in setting up MaxScale are:

* Install the package relevant to your distribution
* Create the required users in your MariaDB or MySQL Replication cluster
* Create a MaxScale configuration file

### Installation

The precise installation process will vary from one distribution to another details of what to do with the RPM and DEB packages can be found on the download site when you select the distribution you are downloading from. The process involves setting up your package manager to include the MariaDB repositories and then running the package manager for your distribution (usually yum or apt-get).

Upon successful completion of the installation command you will have MaxScale installed and ready to be run but without a configuration. You must create a configuration file before you first run MaxScale which is covered in a later section.

### Creating Database Users

MaxScale needs to connect to the backend databases and run queries for two reasons; one to determine the current state of the database and the other to retrieve the user information for the database cluster. The first pair of credentials will be used by the monitor modules and the second is used by MaxScale itself. This may be done either using two separate usernames or with a single user.

The first user required must be able to select data from the table mysql.user, to create this user follow the steps below.

1. Connect to the current master server in your replication tree as the root user
2. Create the user, substituting the username, password and host on which maxscale runs within your environment

```
MariaDB [(none)]> create user '*username*'@'*maxscalehost*' identified by '*password*';

**Query OK, 0 rows affected (0.00 sec)**
```

1. Grant select privileges on the mysql.user table.

```
MariaDB [(none)]> grant SELECT on mysql.user to '*username*'@'*maxscalehost*';

**Query OK, 0 rows affected (0.03 sec)**
```

Additionally, `SELECT` privileges on the `mysql.db` and `mysql.tables_priv` tables and `SHOW DATABASES` privileges are required in order to load databases name and grants suitable for database name authorization.

```
MariaDB [(none)]> GRANT SELECT ON mysql.db TO 'username'@'maxscalehost';

**Query OK, 0 rows affected (0.00 sec)**

MariaDB [(none)]> GRANT SELECT ON mysql.tables_priv TO 'username'@'maxscalehost';

**Query OK, 0 rows affected (0.00 sec)**

MariaDB [(none)]> GRANT SHOW DATABASES ON *.* TO 'username'@'maxscalehost';

**Query OK, 0 rows affected (0.00 sec)**
```

The second user is used to monitored the state of the cluster. This user, which may be the same username as the first, requires permissions to access the various sources of monitoring data. In order to monitor a replication cluster this user must be granted the roles REPLICATION SLAVE and REPLICATION CLIENT

```
MariaDB [(none)]> grant REPLICATION SLAVE on *.* to '*username*'@'*maxscalehost*';

**Query OK, 0 rows affected (0.00 sec)**

MariaDB [(none)]> grant REPLICATION CLIENT on *.* to '*username*'@'*maxscalehost*';

**Query OK, 0 rows affected (0.00 sec)**
```

If you wish to use two different usernames for the two different roles of monitoring and collecting user information then create a different username using the first two steps from above.

### Creating additional grants for users

Because MaxScale is a proxy the backend databases will see all clients as if they were connecting from MaxScale's address. This usually requires users to create additional grants for MaxScale's hostname. The best way to describe this process is with an example.

User `'jdoe'@'192.168.0.200` has the following grant on the cluster: `GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO 'jdoe'@'192.168.0.200'`. When the user connects directly to the server it will see it as `'jdoe'@'192.168.0.200` connecting to the server and it will match the grant for `'jdoe'@'192.168.0.200`.

If MaxScale is at the address `192.168.0.101` and the user `jdoe` connects to this MaxScale, the backend server will see the connection as `'jdoe'@'192.168.0.101'`. Since the backend server has no grants for `'jdoe'@'192.168.0.101'`, the connection from MaxScale to the server will be refused.

We can fix this by either creating a matching grant for user `jdoe` from the MaxScale address or by using a wildcard to cover both addresses.

The quickest way to do this is by doing a SHOW GRANTS query:

```
MariaDB [(none)]> SHOW GRANTS FOR 'jdoe'@'192.168.0.200';
+-----------------------------------------------------------------------+
| Grants for jdoe@192.168.0.200                                         |
+-----------------------------------------------------------------------+
| GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO 'jdoe'@'192.168.0.200' |
+-----------------------------------------------------------------------+
1 row in set (0.01 sec)
```

Then creating the user `'jdoe'@'192.168.0.101'` and giving it the same grants:

```
MariaDB [(none)]> CREATE USER 'jdoe'@'192.168.0.101';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO 'jdoe'@'192.168.0.101';
Query OK, 0 rows affected (0.00 sec)
```

The other option is to use a wildcard grant like `GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO 'jdoe'@'%'`. This is more convenient but also less secure than having specific grants for both the client's address and MaxScale's address.

### Creating the configuration file

The configuration file creation is covered in different tutorials.

#### Master-Slave cluster

* [MySQL Replication Connection Routing Tutorial](maxscale-connection-routing-with-mysql-replication.md)
* [MySQL Replication Read-Write Splitting Tutorial](maxscale-readwrite-splitting-with-mysql-replication.md)

#### Galera cluster

* [Galera Cluster Connection Routing Tutorial](maxscale-connection-routing-with-galera-cluster.md)
* [Galera Cluster Read Write Splitting Tutorial](maxscale-readwrite-splitting-with-galera-cluster.md)

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
