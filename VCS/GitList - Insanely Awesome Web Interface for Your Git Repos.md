# GitList - Insanely Awesome Web Interface for Your Git Repos

Almost 80-90 people visit [How To: Install and Configure GitWeb](http://gofedora.com/how-to-install-configure-gitweb/)
everyday in search of setting up a web interface for their git repositories. Though gitweb is nice, it’s a bit 
painful to setup and the web interface is not that appealing. The other day I received this email from Klaus Silveira.

> Hello Kulbir,
>I saw your article about installing Gitweb and i decided to send this shameless self-promotion. Maybe you could try my open-source project, GitList https://github.com/klaussilveira/gitlist
>
>I’m looking for beta testers and supporters. FLOSS. :)

So, I thought I’ll just give it a try. Today, I got it working and was blown away by the amazing interface! 
It’s almost like a super simplified version of GitHub. I was so impressed that I immediately setup a demo website at 
git.gofedora.com for others to look at and fall in love :-)Another good thing about GitList is that it’s very simple to setup.
Below is a step by step process to install and configure GitList to expose your public Git repositories to the internet.

### What You Need?

You need the following packages before you can setup GitList.

- Apache (with mod_rewrite)

- Git

- PHP

### Installing Required Packages

Most modern operating systems have the above mentioned packages installed by default. Even if you don’t have them already, 
you can use your OS package manager to install them quickly. To install on Fedora/RedHat/CentOS using yum, 
use the following command:

```bash
[root@whitemagnet.com ~]$ yum install php git httpd
```

For Ubuntu/Debian, use the following command:

```bash
[root@whitemagnet.com ~]$ apt-get install php git apache2
```

### Assumptions

For setting up GitList, I am assuming the following directory paths and other variables.

- Path to public Git repositories : /home/saini/code/public/
- Path to Apache document root : /var/www/html/
- Path to Git executable : /usr/bin/git (Use “which git” to find out for your OS)
- Web URL for browsing git repos : mygit.example.com/gitlist/

### Installing and Configuring GitList

Follow the following simple steps to install and configure GitList.

**Step 1** : Clone GitList repository from GitHub to /var/www/html/gitlist/

```bash
[root@whitemagnet.com ~]$ cd /var/www/html/
[root@whitemagnet.com html]$ git clone git://github.com/klaussilveira/gitlist.git gitlist
```

**Step 2** : Create cache directory and make it globally writable

```bash
[root@whitemagnet.com ~]$ cd gitlist
[root@whitemagnet.com gitlist]$ mkdir cache
[root@whitemagnet.com gitlist]$ chmod 777 cache
```

**Step 3** : Configure GitList using `config.ini`

Open the config.ini (in gitlist directory) and set the option properly. Refer the sample shown below.

```ini
[git]
client = '/usr/bin/git' ; Your git executable path
repositories = '/home/saini/code/public/' ; Path to your repositories (with ending slash)
 
[app]
baseurl = 'http://mygit.exmaple.com/gitlist' ; Base URL of the application (without ending slash)
```

**Step 4** : Make sure your Apache can read your `.htaccess` file in gitlist directory

GitList utilizes Apache’s `mod_rewrite` module to provide nice URLs. Make sure your Apache is configured to 
read .htaccess from the gitlist directory. Open your Apache config file (generally located at 
/etc/httpd/conf/httpd.conf or /etc/apache2/ports.conf) and look for the following

```xml
<Directory "/var/www/html">
```

In this segment, make sure you have AllowOverride All as below.

```xml
<Directory "/var/www/html">
# Other lines omitted
AllowOverride All
# Other lines omitted
</Directory>
```

**Step 5** : Reload or restart Apache daemon if needed

```bash
[root@whitemagnet.com ~]$ apachctl -k restart (or apache2ctl -k restart for Ubuntu/Debian)
```

**Step 6** : Get some sample repositories in your public repo directory

Get some sample repositories in your public repo directory from GitHub.

```bash
[root@whitemagnet.com ~]$ cd /home/saini/code/public/
[root@whitemagnet.com ~]$ git clone git://github.com/kulbirsaini/intelligentmirror.git
[root@whitemagnet.com ~]$ git clone git://github.com/kulbirsaini/Railscasts-Sync.git
[root@whitemagnet.com ~]$ git clone git://github.com/zilkey/active_hash.git
```

That’s all! Now, go to http://mygit.example.com/gitlist to discover your public git repos via a cool web interface! 
Leave a comment if you face any issues.

# 链接

[GitList - Insanely Awesome Web Interface for Your Git Repos](https://gofedora.com/insanely-awesome-web-interface-git-repos/)
