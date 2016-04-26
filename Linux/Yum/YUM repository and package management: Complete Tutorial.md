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




# 链接

[YUM repository and package management: Complete Tutorial](http://www.slashroot.in/yum-repository-and-package-management-complete-tutorial)
