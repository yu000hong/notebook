# Adding, Enabling, and Disabling a Yum Repository

This section explains how to add, enable, and disable a repository by using the `yum-config-manager` command.

### Adding a Yum Repository

To define a new repository, you can either add a **[repository]** section to the `/etc/yum.conf` file, or to a **.repo** file 
in the `/etc/yum.repos.d/` directory. All files with the .repo file extension in this directory are read by yum, 
and it is recommended to define your repositories here instead of in /etc/yum.conf.

> **`Be careful when using untrusted software sources`**

> Obtaining and installing software packages from unverified or untrusted software sources other than 
> Red Hat Network constitutes a potential security risk, and could lead to security, stability, compatibility, 
> and maintainability issues.

Yum repositories commonly provide their own .repo file. To add such a repository to your system and enable it, 
run the following command as root:

```bash
yum-config-manager --add-repo repository_url
```

where repository_url is a link to the .repo file. For example, to add a repository located at 
http://www.example.com/example.repo, type the following at a shell prompt:

```bash
~]# yum-config-manager --add-repo http://www.example.com/example.repo
Loaded plugins: product-id, refresh-packagekit, subscription-manager
adding repo from: http://www.example.com/example.repo
grabbing file http://www.example.com/example.repo to /etc/yum.repos.d/example.repo
example.repo                                             |  413 B     00:00
repo saved to /etc/yum.repos.d/example.repo
```

###⁠ Enabling a Yum Repository

To enable a particular repository or repositories, type the following at a shell prompt as root:

```bash
yum-config-manager --enable repository
```

where repository is the unique repository ID (use `yum repolist all` to list available repository IDs). 
Alternatively, you can use a glob expression to enable all matching repositories:

```bash
yum-config-manager --enable glob_expression…
```

For example, to enable repositories defined in the `[example]`, `[example-debuginfo]`, and `[example-source]`
sections, type:

```bash
~]# yum-config-manager --enable example\*
Loaded plugins: product-id, refresh-packagekit, subscription-manager
============================== repo: example ==============================
[example]
bandwidth = 0
base_persistdir = /var/lib/yum/repos/x86_64/6Server
baseurl = http://www.example.com/repo/6Server/x86_64/
cache = 0
cachedir = /var/cache/yum/x86_64/6Server/example
[output truncated]
```

When successful, the `yum-config-manager --enable` command displays the current repository configuration.

### Disabling a Yum Repository

To disable a Yum repository, run the following command as root:

```bash
yum-config-manager --disable repository
```

where repository is the unique repository ID (use `yum repolist all` to list available repository IDs). 
Similarly to yum-config-manager --enable, you can use a glob expression to disable all matching repositories at the same time:

```bash
yum-config-manager --disable glob_expression
```

When successful, the `yum-config-manager --disable` command displays the current configuration.

# 链接

[Adding, Enabling, and Disabling a Yum Repository](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Managing_Yum_Repositories.html)
