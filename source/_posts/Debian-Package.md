---
title: Debian-Package
date: 2019-06-28 10:10:56
tags: debian, linux
---

Similar to .rpm, .pkg and .dmg, debian package(.deb) is a installation pacakge which is meant for debian OS. It is easy to build, install and interrept. Debian package can be used for file installation and also file compression. This article this a short guide to build a simple debian package and install it to another machine.
The example used in this article is to install a systemd service, which describe in the previous blog, to a new machine.

## Debian Package Structure

Debian packages are standard Unix ar archives that include two tar archives. One archive holds the control information and another contains the installable data.

The following tree diagram shows a very simple structure of .deb package. Inside DEBIAN folder does it store all the control related scripts and information.
For other folders and files rather than DEBIAN folder, it is as if the debian package is the root directory. For example, the intended installation is `/home/han/Documents`, in debian package, just put the file inside folder `example.deb/home/han/Documents`, during installation, the file will be installed to the intended location. As simple as that!

```bash
example.deb
├── DEBIAN
│   ├── conffiles
│   ├── control
│   ├── md5sums
│   ├── preinst
│   └── prerm
├── home
│   └── han
│       └── Documents
└── lib
    └── example
        ├── bin
        └── lib

```

## Pre and Post Scripts

## Build a Package

## Install a Package
