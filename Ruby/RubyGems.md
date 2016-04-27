# RubyGems

RubyGems is a package manager for the Ruby programming language that provides a standard format for distributing Ruby 
programs and libraries (in a self-contained format called a "gem"), a tool designed to easily manage the installation 
of gems, and a server for distributing them. RubyGems was created around November 2003 and is now part of the standard 
library from Ruby version 1.9.

Gems are packages similar to Ebuilds. They contain package information along with files to install.
Gems are usually built from ".gemspec" files, which are [YAML](https://en.wikipedia.org/wiki/YAML) files containing 
information on Gems. However, Ruby code may also build Gems directly. Such a practice is usually used with Rake.

RubyGems is very similar to [apt-get](https://en.wikipedia.org/wiki/Apt-get), 
[portage](https://en.wikipedia.org/wiki/Portage_(software)), 
[yum](https://en.wikipedia.org/wiki/Yellowdog_Updater,_Modified) and 
[npm](https://en.wikipedia.org/wiki/Npm_(software)) in functionality.

### `gem` Command

The `gem` command is used to build, upload, download, and install Gem packages.

**Installation:**

> gem install mygem

**Uninstallation:**

> gem uninstall mygem

**Listing installed gems:**

> gem list --local

**Listing available gems, e.g.:**

> gem list --remote

**Create RDoc documentation for all gems:**

> gem rdoc --all

**Download but do not install a gem:**

> gem fetch mygem

**Search available gems, e.g.:**

> gem search STRING --remote

