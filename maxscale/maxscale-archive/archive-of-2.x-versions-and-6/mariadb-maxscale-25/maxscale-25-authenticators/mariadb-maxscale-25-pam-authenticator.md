# MaxScale 2.5 PAM Authenticator

* [PAM Authenticator](mariadb-maxscale-25-pam-authenticator.md#pam-authenticator)
  * [Configuration](mariadb-maxscale-25-pam-authenticator.md#configuration)
    * [pam\_use\_cleartext\_plugin](mariadb-maxscale-25-pam-authenticator.md#pam_use_cleartext_plugin)
    * [pam\_mode](mariadb-maxscale-25-pam-authenticator.md#pam_mode)
  * [Anonymous user mapping](mariadb-maxscale-25-pam-authenticator.md#anonymous-user-mapping)
  * [Implementation details and limitations](mariadb-maxscale-25-pam-authenticator.md#implementation-details-and-limitations)
    * [Two-factor authentication support](mariadb-maxscale-25-pam-authenticator.md#two-factor-authentication-support)
  * [Test tool](mariadb-maxscale-25-pam-authenticator.md#test-tool)

Pluggable authentication module (PAM) is a general purpose authentication API.\
An application using PAM can authenticate a user without knowledge about the\
underlying authentication implementation. The actual authentication scheme is\
defined in the operating system PAM config (e.g. `/etc/pam.d/`), and can be\
quite elaborate. MaxScale supports a very limited form of the PAM protocol,\
which this document details.

### Configuration

The MaxScale PAM module requires little configuration. All that is required\
is to change the listener authenticator module to "PAMAuth".

```
[Read-Write-Listener]
type=listener
address=::
service=Read-Write-Service
authenticator=PAMAuth

[Master-Server]
type=server
address=123.456.789.10
port=12345
```

MaxScale uses the PAM authenticator plugin to authenticate users with _plugin_\
set to "pam" in the _mysql.user_-table. The PAM service name of a user is read\
from the _authetication\_string_-column. The matching PAM service in the\
operating system PAM config is used for authenticating the user. If th&#x65;_&#x61;uthetication\_string_ for a user is empty, the fallback service "mysql" is\
used.

PAM service configuration is out of the scope of this document, see[The Linux-PAM System Administrators' Guide](https://www.linux-pam.org/Linux-PAM-html/Linux-PAM_SAG.html)\
for more information. A simple service definition used for testing this module\
is below.

```
auth            required        pam_unix.so
account         required        pam_unix.so
```

#### `pam_use_cleartext_plugin`

Boolean, default value is "false". If enabled, MaxScale communicates with the\
client as if using[mysql\_clear\_password](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/clients-and-utilities/server-client-software/client-libraries/clientserver-protocol/1-connecting/connection#mysql_clear_password-plugin).\
This setting has no effect on MaxScale-to-backend communication, which adapts to\
either "dialog" or "mysql\_clear\_password", depeding on which one the backend\
suggests. This setting is meant to be used with the similarly named MariaDB\
Server setting.

```
authenticator_options=pam_use_cleartext_plugin=1
```

#### `pam_mode`

This setting defines the authentication mode used. Two values are supported:

* `password` (default) Normal password-based authentication
* `password_2FA` Password + 2FA-code based authentication

```
authenticator_options=pam_mode=password_2FA
```

If set to "password\_2FA", any users authenticating via PAM will be asked two\
passwords ("Password" and "Verification code") during login. MaxScale uses the\
normal password when either the local PAM api or a backend asks for "Password".\
MaxScale answers any other password prompt (e.g. "Verification code") with the\
second password. See[the limitations section](mariadb-maxscale-25-pam-authenticator.md#implementation-details-and-limitations)\
for more details. Two-factor mode is incompatible wit&#x68;_&#x70;am\_use\_cleartext\_plugin_.

### Anonymous user mapping

The MaxScale PAM authenticator supports a limited version of[user mapping](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/user-and-group-mapping-with-pam).\
It requires less configuration but is also less accurate than the server\
authentication. Anonymous mapping is enabled in MaxScale if the following user\
exists:

* Empty username (e.g. `''@'%'` or `''@'myhost.com'`)
* `plugin = 'pam'`
* Proxy grant is on (The query `SHOW GRANTS FOR user@host;` returns at least one\
  row with `GRANT PROXY ON ...`)

When the authenticator detects such users, anonymous account mapping is enabled\
for the hosts of the anonymous users. To verify this, enable the info log\
(`log_info=1` in MaxScale config file) and look for messages such as "Found 2\
anonymous PAM user(s) ..." and "Added anonymous PAM user ..." during MaxScale\
startup.

When mapping is on, the MaxScale PAM authenticator does not require client\
accounts to exist in the `mysql.user`-table received from the backend. MaxScale\
only requires that the hostname of the incoming client matches the host field of\
one of the anonymous users (comparison performed using `LIKE`). If a match is\
found, MaxScale attempts to authenticate the client to the local machine with\
the username and password supplied. The PAM service used for authentication is\
read from the `authentication_string`-field of the anonymous user. If\
authentication was successful, MaxScale then uses the username and password to\
log to the backends.

Anonymous mapping is only attempted if the client username is not found in the`mysql.user`-table as explained in [Configuration](mariadb-maxscale-25-pam-authenticator.md#configuration). This means,\
that if a user is found and the authentication fails, anonymous authentication\
is not attempted even when it could use a different PAM service with a different\
outcome.

Setting up PAM group mapping for the MariaDB server is a more involved process\
as the server requires details on which Unix user or group is mapped to which\
MariaDB user. See[this guide](https://app.gitbook.com/s/SsmexDFPv2xG2OTyO5yV/reference/plugins/authentication-plugins/authentication-with-pluggable-authentication-modules-pam/configuring-pam-authentication-and-user-mapping-with-unix-authentication)\
for more details. Performing all the steps in the guide also on the MaxScale\
machine is not required, as the MaxScale PAM plugin only checks that the client\
host matches an anonymous user and that the client (with the username and\
password it provided) can log into the local PAM configuration. If using normal\
password authentication, simply generating the Unix user and password should be\
enough.

### Implementation details and limitations

The general PAM authentication scheme is difficult for a proxy such as MaxScale.\
An application using the PAM interface needs to define a _conversation function_\
to allow the OS PAM modules to communicate with the client, possibly exchanging\
multiple messages. This works when a client logs in to a normal server, but not\
with MaxScale since it needs to autonomously log into multiple backends. For\
MaxScale to successfully log into the servers, the messages and answers need to\
be predefined. The passwords given to MaxScale need to work as is when MaxScale\
logs into the backends. This requirement prevents the use of one-time passwords.

The MaxScale PAM authentication module supports two password modes. In normal\
mode, client authentication begins with MaxScale sending an\
AuthSwitchRequest packet. In addition to the command, the packet contains the\
client plugin name ("dialog" or "mysql\_clear\_password"), a message type byte (4)\
and the message "Password: ". In the next packet, the client should send the\
password, which MaxScale will forward to the PAM api running on the local\
machine. If the password is correct, an OK packet is sent to the client. If the\
local PAM api asks for additional credentials as is typical in two-factor\
authentication schemes, authentication fails. Informational messages such as\
password expiration notifications are allowed. These are simply printed to the\
log.

On the backend side, MaxScale expects the servers to act as MaxScale did towards\
the client. The servers should send an AuthSwitchRequest packet as defined\
above, MaxScale responds with the password received by the client authenticator\
and finally backend replies with OK. Informational messages from backends are\
only printed to the info-log.

#### Two-factor authentication support

MaxScale supports a limited form of two-factor authentication with the`pam_mode=password_2FA`-option. Since MaxScale uses the 2FA-code given by the\
client to log in to the local PAM api as well as all the backends, the code must\
be reusable. This prevents the use of any kind of centrally checked one-use\
codes. Time-based codes work, assuming the backends are checking the codes\
independently of each other. Automatic reconnection features (e.g.\
readwritesplit-router) will not work, as the code has likely changed since\
original authentication.

Optionally, the PAM configuration on the backend servers can be weakened such\
that the servers only asks for the normal password. This way, MaxScale will\
check the 2FA-code of the incoming client, while MaxScale logs into the backends\
using only the password.

Due to technical reasons, MaxScale does not forward the password prompts from\
the PAM api to the client. MaxScale will always ask for "Password" and\
"Verification code", even if the PAM api asks for other items. This prevents the\
use of authentication schemes where a specific question must be answered (e.g.\
"Input code Nr. 5"). This is not a significant limitation, as such schemes would\
not work with backend servers anyway.

### Test tool

MaxScale binary directory contains the _test\_pam\_login_-executable. This simple\
program asks for a username, password and PAM service and then uses the given\
credentials to login to the given service. _test\_pam\_login_ uses the same code\
as MaxScale itself to communicate with the OS PAM interface and may be useful\
for diagnosing PAM login issues.

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
