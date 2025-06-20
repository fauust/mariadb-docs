# Node.js Connection Options

* [Essential options](node-js-connection-options.md#essential-option)
* [Support for big integer](node-js-connection-options.md#support-for-big-integer)
* [Ssl](node-js-connection-options.md#ssl)
* [Configuration](node-js-connection-options.md#configuration)
* [Certificate validation](node-js-connection-options.md#certificate-validation)
* [One-way SSL authentication](node-js-connection-options.md#one-way-ssl-authentication)
* [Two-way SSL authentication](node-js-connection-options.md#two-way-ssl-authentication)
* [Other options](node-js-connection-options.md#other-options)
* [F.A.Q.](node-js-connection-options.md#faq)

### Essential Options

| Option         | Description                                                                                                                                                                                                                                  | Type    | Default     |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------- | ----------- |
| Option         | Description                                                                                                                                                                                                                                  | Type    | Default     |
| user           | User to access database.                                                                                                                                                                                                                     | string  |             |
| password       | User password.                                                                                                                                                                                                                               | string  |             |
| host           | IP address or DNS of database server. Not used when using the socketPath option.                                                                                                                                                             | string  | "localhost" |
| port           | Database server port number.                                                                                                                                                                                                                 | integer | 3306        |
| database       | Default database to use when establishing the connection.                                                                                                                                                                                    | string  |             |
| socketPath     | Permit connecting to the database via Unix domain socket or named pipe, if the server allows it.                                                                                                                                             | string  |             |
| compress       | Compress exchanges with database using gzip. This can give you better performance when accessing a database in a different location.                                                                                                         | boolean | false       |
| connectTimeout | Connection timeout in milliseconds.                                                                                                                                                                                                          | integer | 10 000      |
| socketTimeout  | Socket timeout in milliseconds after the connection is established.                                                                                                                                                                          | integer | 0           |
| rowsAsArray    | Return resultsets as array, rather than a JSON object. This is a faster way to get results. For more information, see the [Promise](connector-nodejs-promise-api.md) and [Callback](connector-nodejs-callback-api.md) query implementations. | boolean | false       |

### Big Integer Support

Integers in JavaScript use IEEE-754 representation. This means that Node.js cannot exactly represent integers in the ±9,007,199,254,740,991 range. However, MariaDB does support larger integers.

This means that when the value set on a column is not in the [safe](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number/isSafeInteger) range, the default implementation receives an inexact representation of the number.

The Connector provides two options to address this issue.

| Option            | Description                                                                                                                          | Type     | Default |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------ | -------- | ------- |
| Option            | Description                                                                                                                          | Type     | Default |
| bigNumberStrings  | When an integer is not in the safe range, the Connector interprets the value as a string                                             | boolean. | false   |
| supportBigNumbers | When an integer is not in the safe range, the Connector interprets the value as a [Long](https://www.npmjs.com/package/long) object. | boolean  | false   |

### SSL

The Connector can encrypt data during transfer using the Transport Layer Security (TLS) protocol. TLS/SSL allows for transfer encryption, and can optionally use identity validation for the server and client.

{% hint style="info" %}
The term SSL (Secure Sockets Layer) is often used interchangeably with TLS, although strictly-speaking the SSL protocol is the predecessor of TLS, and is not implemented as it is now considered insecure.
{% endhint %}

There are two different kinds of SSL authentication:

* One-Way SSL Authentication: The client verifies the certificate of the server. This allows you to encrypt all exchanges and make sure that you are connecting to the expected server (to avoid a man-in-the-middle attack).
* Two-Way SSL Authentication The client verifies the certificate of the server, the server verifies the certificate of the client. This is also called mutual authentication or client authentication. When using this system, the client also requires a dedicated certificate.

#### Server Configuration

In order to use SSL, you need to ensure that the MariaDB Server is correctly configured. You can determine this using the [have\_ssl](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption/ssltls-system-variables#have_ssl) system variable.

```sql
SHOW VARIABLES LIKE 'have_ssl';
```

```
+---------------+----------+
| Variable_name | Value    |
+---------------+----------+
| have_ssl      | DISABLED |
+---------------+----------+
```

A value of NO indicates that MariaDB was compiled without support for TLS. DISABLED means that it was compiled with TLS support, but it's currently turned off. In order to use SSL with the Connector, the server must return YES, indicating that TLS support is available and turned on.\
For more information, see the [MariaDB Server](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/security/securing-mariadb/securing-mariadb-encryption/data-in-transit-encryption) documentation.

#### User Configuration

Enabling the [ssl option](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/server-management/starting-and-stopping-mariadb/mariadbd-options) on the server, the Connector uses one-way SSL authentication to connect to the server. Additionally, it's recommended that you also configure your users to connect through SSL. This ensures that their accounts can only be used with an SSL connection.

For [GRANT statements](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/grant), use the [REQUIRE SSL](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/grant#per-account-ssltls-options) option for one-way SSL authentication and the [REQUIRE X509](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/grant#per-account-ssltls-options) option for two-way SSL authentication. For more information, see the [CREATE USER](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/create-user) documentation.

```sql
CREATE USER 'johnSmith'@'%' IDENTIFIED BY PASSWORD('passwd');
GRANT ALL ON company.* TO 'johnSmith'@'%' REQUIRE SSL;
```

Now when this user attempts to connect to MariaDB without SSL, the server rejects the connection.

#### Configuration

* ssl: boolean/JSON object.

JSON object:

| Option              | Description                                                                                                                                                                                                               | Type                                                  | Default                  |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | ------------------------ |
| Option              | Description                                                                                                                                                                                                               | Type                                                  | Default                  |
| checkServerIdentity | function(servername, cert) to replace SNI default function                                                                                                                                                                | Function                                              |                          |
| minDHSize           | Minimum size of the DH parameter in bits to accept a TLS connection                                                                                                                                                       | number                                                | 1024                     |
| pfx                 | Optional PFX or PKCS12 encoded private key and certificate chain. Encrypted PFX will be decrypted with passphrase if provided                                                                                             | \*string / string\[] / Buffer / Buffer\[] / Object\[] |                          |
| key                 | Optional private keys in PEM format. Encrypted keys are decrypted with passphrase if provided                                                                                                                             | \*string / string\[] / Buffer / Buffer\[] / Object\[] |                          |
| passphrase          | Optional shared passphrase used for a single private key and/or a PFX                                                                                                                                                     | string                                                |                          |
| cert                | Optional cert chains in PEM format. One cert chain should be provided per private key                                                                                                                                     | string / string\[] / Buffer / Buffer\[]               |                          |
| ca                  | Optionally override the trusted CA certificates. Default is to trust the well-known CAs curated by Mozilla. For self-signed certificates, the certificate is its own CA, and must be provided                             | string / string\[] / Buffer / Buffer\[]               |                          |
| ciphers             | Optional cipher suite specification, replacing the default                                                                                                                                                                | string                                                |                          |
| honorCipherOrder    | Attempt to use the server's cipher suite preferences instead of the client's                                                                                                                                              | boolean                                               |                          |
| ecdhCurve           | A string describing a named curve or a colon separated list of curve NIDs or names, for example P-521:P-384:P-256, to use for ECDH key agreement, or false to disable ECDH. Set to auto to select the curve automatically | string                                                | tls.DEFAULT\_ECDH\_CURVE |
| clientCertEngine    | Optional name of an OpenSSL engine which can provide the client certificate                                                                                                                                               | string                                                |                          |
| crl                 | Optional PEM formatted CRLs (Certificate Revocation Lists)                                                                                                                                                                | string / string\[] / Buffer / Buffer\[]               |                          |
| dhparam             | Diffie Hellman parameters, required for Perfect Forward Secrecy                                                                                                                                                           | string / Buffer                                       |                          |
| secureProtocol      | Optional SSL method to use, default is "SSLv23\_method"                                                                                                                                                                   | string                                                |                          |

The Connector uses the Node.js implementation of TLS. For more information, see the [Node.js TLS API](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) documentation.

#### Certificate Validation

**Trusted CA**

By default, Node.js trusts the well-known root Certificate Authorities (CA), based on Mozilla. For a complete list, (including the popular and free Let's Encrypt), see the [CA Certificate List](https://ccadb-public.secure.force.com/mozilla/IncludedCACertificateReport).

When using a certificate signed with a certificate chain from a root CA known to Node.js, the only configuration you need to do is enable the `ssl` option.

**Certificate Chain Validation**

A certificate chain is a list of certificates that were issued from the same Certification Authority hierarchy. In order for any certificate to be validated, all certificates in the chain have to be validated.

In cases where intermediate or root certificates are not trusted by the Connector, the Connector rejects the connection and issues an error.

**Hostname Verification (SNI)**

Certificates can provide hostname verification to the driver. By default this is done against the certificate's subjectAlternativeName DNS name field.

#### One-way SSL Authentication

When the server certificate is signed using the certificate chain that uses a root CA known in the JavaScript trust store, setting the `ssl` option enables one-way SSL authentication.

For instance,

```js
const mariadb = require('mariadb');
mariadb
 .createConnection({
   host: 'myHost.com', 
   ssl: true, 
   user: 'myUser', 
   password:'MyPwd', 
   database:'db_name'
 }).then(conn => {})
```

When the server uses a self-signed certificate or uses an intermediate certificate, there are two different possibilities:

In non-production environments, you can tell the Connector to trust all certificates by setting rejectUnauthorized to false. Do **NOT** use this in production.

```js
//connecting
mariadb
 .createConnection({
   host: 'myHost.com', 
   ssl: {
	 rejectUnauthorized: false
   }, 
   user: 'myUser', 
   password:'MyPwd', 
 }).then(conn => {})
```

A more secure alternative is to provide the certificate chain to the Connector.

```js
const fs = require("fs");
const mariadb = require('mariadb');

//reading certificates from file
const serverCert = [fs.readFileSync("server.pem", "utf8")];

//connecting
mariadb
 .createConnection({
   user: "myUser",
   host: "myHost.com",
   ssl: {
	 ca: serverCert
   }
 }).then(conn => {})
```

**Using Specific TLS Protocols or Ciphers**

In situations where you don't like the default TLS protocol or cipher or where you would like to use a specific version, you force the Connector to use the one you want using the `secureProtocol` and `cipher` options.

For instance, say you want to connect using TLS version 1.2:

```js
//connecting
mariadb
 .createConnection({ 
   user:"myUser", 
   host: "myHost.com",
   ssl: {
	 ca: serverCert,
	 secureProtocol: "TLSv1_2_method",
	 ciphers:
	   "ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES256-GCM-SHA384:DHE-RSA-AES128-GCM-SHA256"        
   }
 }).then(conn => {})
```

For more information on what's available, see [possible protocol](https://www.openssl.org/docs/man1.0.2/ssl/ssl.html#DEALING-WITH-PROTOCOL-METHODS) values.

#### Two-way SSL Authentication

Mutual SSL authentication or certificate-based mutual authentication refers to two parties authenticating each other by verifying the provided digital certificates. This allows both parties to be assured of the other's identity. In order to use mutual authentication, you must set the [REQUIRE X509](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/grant#per-account-ssltls-options) option in the [GRANT](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/account-management-sql-commands/grant) statement. For example,

```sql
GRANT ALL ON company.* TO 'johnSmith'@'%' REQUIRE X509;
```

This option causes the server to ask the Connector for a client certificate. **If the user is not set with REQUIRE X509, the server defaults to one-way authentication**

When using mutual authentication, you need a certificate, (and its related private key), for the Connector as well as the server. If the Connector doesn't provide a certificate and the user is set to REQUIRE X509, the server returns a basic `Access denied for user` message.

In the event that you would like to see how users are defined, you can find this information by querying the [mysql.user table](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysql-user-table) on the server. For instance, say you wanted information on the johnSmith user.

```sql
SELECT ssl_type, ssl_cipher, x509_subject 
FROM mysql.user
WHERE User = 'johnSmith';
```

You can test it by creating a user with REQUIRE X509 for testing:

```sql
CREATE USER 'X509testUser'@'%';
GRANT ALL PRIVILEGES ON *.* TO 'X509testUser'@'%' REQUIRE X509;
```

Then use its credentials in your application:

```js
const fs = require("fs");
const mariadb = require('mariadb');

//reading certificates from file
const serverCert = [fs.readFileSync("server.pem", "utf8")];
const clientKey = [fs.readFileSync("client.key", "utf8")];
const clientCert = [fs.readFileSync("client.pem", "utf8")];

//connecting
mariadb
 .createConnection({ 
   user:"X509testUser", 
   host: "mariadb.example.com",
   ssl: {
	 ca: serverCert,
	 cert: clientCert,
	 key: clientKey
   }
 }).then(conn => {})
```

**Using Keystores**

Keystores allow you to store private keys and certificate chains encrypted with a password to file. For instance, using OpenSSL you can generate a keystore using PKCS12 format:

```bash
openssl pkcs12 \
	-export \
	-in "${clientCertFile}" \
	-inkey "${clientKeyFile}" \
	-out "${keystoreFile}" \
	-name "mariadbAlias" \
	-passout pass:kspass
```

You can then use the keystore in your application:

```js
const fs = require("fs");
const mariadb = require('mariadb');

//reading certificates from file (keystore must be read as binary)
const serverCert = fs.readFileSync("server.pem", "utf8");
const clientKeystore = fs.readFileSync("keystore.p12");

//connecting
mariadb.createConnection({ 
 user:"X509testUser", 
 host: "mariadb.example.com",
 ssl: {
   ca: serverCert,
   pfx: clientKeystore,
   passphrase: "kspass"
 }
}).then(conn => {});
```

### Other Options

| Option             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Type         | Default              |
| ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | -------------------- |
| Option             | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Type         | Default              |
| charset            | Protocol character set used with the server. It's mainly used for micro-optimizations. The default is often sufficient.                                                                                                                                                                                                                                                                                                                                                                | string       | UTF8MB4\_UNICODE\_CI |
| dateStrings        | Whether to retrieve dates as strings or as Date objects.                                                                                                                                                                                                                                                                                                                                                                                                                               | boolean      | false                |
| debug              | Logs all exchanges with the server. Displays in hexa.                                                                                                                                                                                                                                                                                                                                                                                                                                  | boolean      | false                |
| foundRows          | When enabled, the update number corresponds to update rows. When disabled, it indicates the real rows changed.                                                                                                                                                                                                                                                                                                                                                                         | boolean      | true                 |
| multipleStatements | Allows you to issue several SQL statements in a single query() call. (For example: INSERT INTO a VALUES('b'); INSERT INTO c VALUES('d');). This might be a security risk as it allows SQL Injection attacks.                                                                                                                                                                                                                                                                           | boolean      | false                |
| namedPlaceholders  | Allows the use of named placeholders.                                                                                                                                                                                                                                                                                                                                                                                                                                                  | boolean      | false                |
| permitLocalInfile  | Allows the use of [LOAD DATA INFILE](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile) statements.Loading data from a file from the client may be a security issue, as a man-in-the-middle proxy server can change the actual file the server loads. Being able to execute a query on the client gives you access to files on the client. | boolean      | false                |
| timezone           | Forces use of the indicated timezone, rather than the current Node.js timezone. Possible values are Z for UTC, local or ±HH:MM format                                                                                                                                                                                                                                                                                                                                                  | string       |                      |
| nestTables         | Presents resultsets by table to avoid results with colliding fields. See the query() description for more information.                                                                                                                                                                                                                                                                                                                                                                 | boolean      | false                |
| pipelining         | Sends queries one by one without waiting for the results of the previous entry. For more information, see [Pipelining](connector-nodejs-pipelining.md)                                                                                                                                                                                                                                                                                                                                 | boolean      | true                 |
| trace              | Adds the stack trace at the time of query creation to the error stack trace, making it easier to identify the part of the code that issued the query. Note: This feature is disabled by default due to the performance cost of stack creation. Only turn it on when you need to debug issues.                                                                                                                                                                                          | boolean      | false                |
| typeCast           | Allows you to cast result types.                                                                                                                                                                                                                                                                                                                                                                                                                                                       | function     |                      |
| connectAttributes  | Sends information (client name, version, operating system, Node.js version, and so on) to the [Performance Schema](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/sql-statements-and-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-session_connect_attrs-table). When enabled, the Connector sends JSON attributes in addition to the defaults.                                       | boolean/json | false                |
| metaAsArray        | Compatibility option, causes Promise to return an array object, \[rows, metadata] rather than the rows as JSON objects with a meta property.                                                                                                                                                                                                                                                                                                                                           | boolean      | false                |

### F.A.Q.

**Error Hostname/IP doesn't match certificate's altnames**

Clients verify certificate SAN (subject alternative names) and CN to ensure that the certificate corresponds to the hostname. If the certificate's SAN/CN does not correspond to the host option, it returns an error such as:

```
Hostname/IP doesn't match certificate's altnames: "Host: other.example.com. is not cert's CN: mariadb.example.com"
```

To fix this, correct the host value to correspond to the host identified in the certificate.

**Error routines:ssl\_choose\_client\_version:unsupported protocol**

Since Node.js 12 minimum TLS version is set to 1.2.

MariaDB server can be built with different SSL library, old version supporting only TLS up to 1.1.\
The error "1976:error:1425F102:SSL routines:ssl\_choose\_client\_version:unsupported protocol" can occur if MariaDB SSL implementation doesn't support TLSv1.2.

This can be solved by :

* Server side: update MariaDB to a recent version
* Client side: permit lesser version with "tls.DEFAULT\_MIN\_VERSION = 'TLSv1.1';" or permitting lesser version of protocol by connection configuration: using option \`ssl: { secureProtocol: 'TLSv1\_1\_method' }'

{% @marketo/form formId="4316" %}
