# YUM repository and package management: Complete Tutorial

![](http://www.slashroot.in/sites/default/files/styles/article_image_full_node/public/field/image/yum%20package_0.png?itok=ys98Ee-n)

Every operating system must have some or the other way to install a program. What's important is the fact that 
the user must not be given the responsibility of managing the overhead involved in the installation of the program.
You would ask, what's the overhead involved in installing a program? Yes there are several overheads involved in 
installing a program in a computer Like the following.

- Is the program compatible with my computer architecture.
- Is that program compatible with my operating system version.
- Does all the programs and libraries required to run a certain program there in the system.
- Will the newly installed program conflict with an already installed program.

An installer or program manager, must handle those overhead by itself by not harassing the user. Linux, by nature is the 
best operating system out there(if you configured it the right waycheeky). The main issue with installing a program's 
in Linux distribution's is the fact that, different distribution's use different methods to install a program.

Here, in this post we will be discussing a very famous tool used to install program's in Red Hat Linux system's(even 
fedora,centos and all red hat like system's). That's none other than the very famous `YUM`.

### Introduction to YUM

YUM stands for **Yellowdog Updater, Modified**. Like all other program's in Linux, YUM is also an open source tool.
It was initially used in Duke University, for managing package installation on their Red Hat based system's. 
These day's its been widely used by almost all Red Hat based system's. In fact its the default program installer 
and package management tool these days.

If you are interested in visiting the official home page of YUM, then i would recommend, visiting the below link of Duke university.

[http://linux.duke.edu/projects/yum/](http://linux.duke.edu/projects/yum/)

### What are packages in Linux?

Red hat Linux,Fedora & all other red hat based distributions uses `RPM` as their main software installation package tool.
A Linux software package is nothing but a compressed archive of files, consisting of a particular product information,
program files, icons, libraries etc. which enables the functioning of that software package.

RPM is the default package installation tool used in Red Hat Linux. RPM stands for **`Red Hat Package Manager`**.

All required files of an application is compiled in a single file format called with a file extension of **.rpm**. 
The Red Hat package manager tool, which is installed in all RPM based system, knows how to open and install these **.rpm** 
files in the system(.rpm files are compiled and are ready to install, they also perform a dependency prerequisite check 
for all required libraries before installing a particular package.)

RPM itself is a vast topic, so we will be covering that in separate post, in great detail.

### So if RPM is already there, why was YUM made?

RPM and YUM are completely two different things. RPM is the package manager tool which installs the package. 
YUM is a repository management tool which will fetch the appropriate package for your particular version of 
Linux(along with all other required packages).

Repositories is an organized collection of packages that YUM uses. YUM can use these repositories to fetch the 
correct and exact version of a particular package compatible for your system. Previously before YUM(or before the 
existence of such repository management tools), the user had to fetch the rpm package for installation, and if a 
dependency problem arises, the user had to fetch those dependencies from internet or some other sources.

YUM will contain the URL's(Uniform Resource Locators) of different repositories in its configuration files. 
You can in fact update all the installed applications on your system, with the help of a single YUM command(yum 
will fetch different packages from appropriate different repositories.)

### Let's create a yum repository for better understanding

Let's create a yum repository from the packages that we have in the Red Hat/Centos installation DVD. Creating a YUM repository will help you to understand the concept of a YUM repository closely. You get a large number of packages(development tools & application packages etc), inside the installation disk. However all are not installed, when you install the operating system.

Later on if you need a particular package, its not at all advisable to insert the installation disk once again, and fetch that required .rpm package and install it. Again if you face dependency problems, you need to fetch that dependency package once again(sometimes there are yet another dependency package required for installing your dependency packagecheeky. So it becomes a tedious job).

Let's go through a step by step method of creating a **`local YUM repository`**.

**`Step 1`**: Copy all the .rpm packages from your Installation disk(also all your collected packages) to an isolated folder on your system.

**`Step 2`**: For showing you this example, i will be copying all the .rpm packages inside `/var/yum` folder

```bash
[root@slashroot2 yum]# pwd
/var/yum
[root@slashroot2 yum]# ls
a2ps-4.13b-57.2.el5.i386.rpm
acl-2.2.39-3.el5.i386.rpm
acpid-1.0.4-7.el5.i386.rpm
adaptx-0.9.13-3jpp.1.i386.rpm
adaptx-doc-0.9.13-3jpp.1.i386.rpm
```

If you see the above output(there are around 2000 packages in the installation disk, i have not shown the whole output.), all packages from the installation disk are copied inside /var/yum. As i told before, a repository is nothing but a collection of packages in a directory. YUM was made, so that an operating system can use different repositories at the same time.

It is not at all feasible for an operating system to download the entire repository(because a repository is sometimes very large in the size of Gigabytes. And YUM was designed to fetch and download only those packages that are required to install your required software on demand.) to install the packages required. For example, if i want to install a package called "Perl",  YUM must first have the list of all the package's in a repository(note the fact that it only requires the list, not the package)

YUM will download the total list of packages available in a repository(the list will contain the package names in the repository, package details etc). Not that it will download only the list of packages with details, not the packages. After downloading the list, If yum was able to fetch all the dependencies for your required package(from that repository or other repositories) yum will install it after confirming with you.

Now lets make that file, which will be containing the package names and other repository details. For this, there is another tool called "`Createrepo`" . Let's see what createrepo does.

```bash
[root@localhost var]# createrepo /var/yum/
2669/2669 - orca-1.0.0-5.el5.i386.rpm
Saving Primary metadata
Saving file lists metadata
Saving other metadata
[root@localhost var]#
```

In the above example, i have ran createrepo command with the directory "/var/yum" as an argument(you need to install "createrepo" package for that command. You will get that in the installation disk, so install it with "`rpm -ivh createrepo-XX-XX-XX.noarch.rpm`").

```bash
[root@localhost yum]# ll | grep ^d
drwxr-xr-x 2 root root      4096 Feb 10 16:40 repodata
[root@localhost yum]#
```

After running "createrepo" for our repository directory you will have an extra directory along with the packages inside the repository. this directory is named as "`repodata`"

Lets see what's inside that directory.

```bash
[root@localhost repodata]# pwd
/var/yum/repodata
[root@localhost repodata]# ll
total 13840
-rw-r--r-- 1 root root  3021610 Feb 10 16:39 filelists.xml.gz
-rw-r--r-- 1 root root 10148949 Feb 10 16:39 other.xml.gz
-rw-r--r-- 1 root root   969664 Feb 10 16:39 primary.xml.gz
-rw-r--r-- 1 root root      951 Feb 10 16:40 repomd.xml
[root@localhost repodata]#
```

You can clearly see there are four files inside that directory. Let's understand the contents of each and every file in detail.

### what is filelists.xml.gz in YUM?

Let's see what's inside that compressed file with the help of "zcat". For explanation i have copied one line from the file "filelists.xml.gz".

```xml
<package pkgid="deee52b24486906ee52576ee471b57061ccd5544" name="php-mbstring" arch="i386">
  <version epoch="0" ver="5.1.6" rel="32.el5"/>
  <file>/etc/php.d/mbstring.ini</file>
  <file>/usr/lib/php/modules/mbstring.so</file>
</package>
```

If you see the above line, the first entry tell's the package ID, which will uniquely identify the package. The second entry "name" of course suggests the name of the package.

### what is primary.xml.gz in YUM?

### What is repomd.xml in yum?

### What is other.xml.gz in yum?

### How to enable the YUM repo, i just created with createrepo for my system?

# 链接

[YUM repository and package management: Complete Tutorial](http://www.slashroot.in/yum-repository-and-package-management-complete-tutorial)
