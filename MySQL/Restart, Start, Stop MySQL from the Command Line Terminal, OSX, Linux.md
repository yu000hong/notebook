# Restart, Start, Stop MySQL from the Command Line Terminal, OSX, Linux

To restart, start or stop MySQL server from the command line, type the following at the shell prompt…

### On Linux start/stop/restart from the command line:

```bash
/etc/init.d/mysqld start
```

```bash
/etc/init.d/mysqld stop
```

```bash
/etc/init.d/mysqld restart
```

Some `Linux` flavours offer the service command too

```bash
service mysqld start
```

```bash
service mysqld stop
```

```bash
service mysqld restart
```

### On OS X to start/stop/restart MySQL pre 5.7 from the command line:

```bash
sudo /usr/local/mysql/support-files/mysql.server start
```

```bash
sudo /usr/local/mysql/support-files/mysql.server stop
```

```bash
sudo /usr/local/mysql/support-files/mysql.server restart
```

### On OS X Yosemite/El Capitan to start/stop/restart MySQL post 5.7  from the command line:

```bash
sudo launchctl load -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

```bash
sudo launchctl unload -F /Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
```

# 链接

[Restart, Start, Stop MySQL from the Command Line Terminal, OSX, Linux](https://coolestguidesontheplanet.com/start-stop-mysql-from-the-command-line-terminal-osx-linux/)
