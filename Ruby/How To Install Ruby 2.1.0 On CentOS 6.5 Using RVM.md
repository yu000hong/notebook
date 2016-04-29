# How To Install Ruby 2.1.0 On CentOS 6.5 Using RVM

### Introduction

Whether you are preparing your VPS to try out a new application, or find yourself in need of a solid and isolated Ruby 
installation, getting your system ready-for-work (inline with CentOS design ideologies of stability, along with its 
incentives of minimalism) can get you feeling a little bit lost.

In this DigitalOcean article, we are focusing on the simplest and quickest rock-solid way to get the latest Ruby 
interpreter (version 2.1.0) installed on a VPS running CentOS 6.5 using the `Ruby Version Manager` - `RVM`.

### Ruby Version Manager (RVM)

Ruby Version Manager, or RVM (and rvm as a command) for short, lets developers and system administrators quickly get started using Ruby and/or developing applications with a Ruby interpreter.

Not only does RVM support multiple versions of Ruby simultaneously, but also it comes with built-in tools to create and work with virtual environments called gemsets. With the help of RVM, it is possible to create any number of perfectly isolated - and self-contained - gemsets where dependencies, packages, and the default Ruby installation are crafted to match your needs and kept accordingly between different stages of deployment -- guaranteed to work the same way regardless of where.
