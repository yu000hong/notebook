# Configuring Web Proxy on CentOS

If your internet connection is behind a web proxy, you need to configure the following on your CentOS server:

System-wide proxy settings - add the following lines to your `/etc/environment` file:

```bash
# vi /etc/environment

http_proxy="http://proxysrv:8080/"
https_proxy="https://proxysrv:8080/"
ftp_proxy="ftp://proxysrv:8080/"
no_proxy=".mylan.local,.domain1.com,host1,host2"
```

To apply these settings without restarting the machine run the following commands on the bash shell:

```bash
export http_proxy="http://proxysrv:8080/"
export https_proxy="https://proxysrv:8080/"
export ftp_proxy="ftp://proxysrv:8080/"
export no_proxy=".mylan.local,.domain1.com,host1,host2"
```

You also need to configure `yum`:

```bash
# vi /etc/yum.conf

proxy=http://proxysrv:8080/
```

# 链接

[Configuring Web Proxy on CentOS](http://www.thesysadminhimself.com/2013/08/configuring-web-proxy-on-centos.html)
