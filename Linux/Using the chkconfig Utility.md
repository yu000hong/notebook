# Using the chkconfig Utility

The **`chkconfig`** utility is a command-line tool that allows you to specify in which **runlevel** to start a selected 
service, as well as to list all available services along with their current setting. Note that with the exception of listing, 
you must have superuser privileges to use this command.

### Listing the Services

To display a list of system services (services from the `/etc/rc.d/init.d/` directory, as well as the services controlled 
by xinetd), either type `chkconfig --list`, or use chkconfig with no additional arguments. You will be presented with an 
output similar to the following:

```bash
~]# chkconfig --list
NetworkManager  0:off   1:off   2:on    3:on    4:on    5:on    6:off
abrtd           0:off   1:off   2:off   3:on    4:off   5:on    6:off
acpid           0:off   1:off   2:on    3:on    4:on    5:on    6:off
anamon          0:off   1:off   2:off   3:off   4:off   5:off   6:off
atd             0:off   1:off   2:off   3:on    4:on    5:on    6:off
auditd          0:off   1:off   2:on    3:on    4:on    5:on    6:off
avahi-daemon    0:off   1:off   2:off   3:on    4:on    5:on    6:off
... several lines omitted ...
wpa_supplicant  0:off   1:off   2:off   3:off   4:off   5:off   6:off

xinetd based services:
        chargen-dgram:  off
        chargen-stream: off
        cvs:            off
        daytime-dgram:  off
        daytime-stream: off
        discard-dgram:  off
... several lines omitted ...
        time-stream:    off
```

Each line consists of the name of the service followed by its status (on or off) for each of the seven numbered runlevels. 
For example, in the listing above, NetworkManager is enabled in runlevel 2, 3, 4, and 5, while abrtd runs in runlevel 3 and 5. 
The `xinetd` based services are listed at the end, being either on, or off.

To display the current settings for a selected service only, use chkconfig --list followed by the name of the service:

```bash
chkconfig --list service_name
```

For example, to display the current settings for the `sshd` service, type:

```bash
~]# chkconfig --list sshd
sshd            0:off   1:off   2:on    3:on    4:on    5:on    6:off
```

You can also use this command to display the status of a service that is managed by xinetd. In that case, 
the output will only contain the information whether the service is enabled or disabled:

```bash
~]# chkconfig --list rsync
rsync           off
```

### Enabling a Service

To enable a service in runlevels 2, 3, 4, and 5, type the following at a shell prompt as root:

```bash
chkconfig service_name on
```

For example, to enable the `httpd` service in these four runlevels, type:

```bash
~]# chkconfig httpd on
```

To enable a service in certain runlevels only, add the **--level** option followed by numbers from 0 to 6 
representing each runlevel in which you want the service to run:

```bash
chkconfig service_name on --level runlevels
```

For instance, to enable the abrtd service in runlevels 3 and 5, type:

```bash
~]# chkconfig abrtd on --level 35
```

The service will be started the next time you enter one of these runlevels. If you need to start the service immediately, 
use the service command as described in Section 11.3.2, “Starting a Service”. **Do not** use the --level option when working 
with a service that is managed by xinetd, as it is not supported. For example, to enable the `rsync` service, type:

```bash
~]# chkconfig rsync on
```

If the `xinetd` daemon is running, the service is immediately enabled without having to manually restart the daemon.

### Disabling a Service

To disable a service in runlevels 2, 3, 4, and 5, type the following at a shell prompt as root:

```bash
chkconfig service_name off
```

For instance, to disable the `httpd` service in these four runlevels, type:

```bash
~]# chkconfig httpd off
```

To disable a service in certain runlevels only, add the **--level** option followed by numbers from 0 to 6 
representing each runlevel in which you do not want the service to run:

```bash
chkconfig service_name off --level runlevels
```

For instance, to disable the `abrtd` in runlevels 2 and 4, type:

```bash
~]# chkconfig abrtd off --level 24
```

The service will be stopped the next time you enter one of these runlevels. If you need to stop the service immediately, 
use the service command as described in Section 11.3.3, “Stopping a Service”. **Do not** use the --level option when 
working with a service that is managed by `xinetd`, as it is not supported. For example, to disable the rsync service, type:

```bash
~]# chkconfig rsync off
```

If the xinetd daemon is running, the service is immediately disabled without having to manually restart the daemon.

# 链接

[Using the chkconfig Utility](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/s2-services-chkconfig.html)

