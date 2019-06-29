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

To install a systemd service, the files should be in the directory `/lib/systemd/system`. So accoridng to the debian package structure, we should save the origial file under `./installer/lib/systemd/system/example.service`

```bash
        #!/bin/bash
        #Path: ./installer/home/han/test_service.bash
        /usr/bin/java -jar /home/structo/HelloWorld.jar
        while :
        do
        echo "Looping...";
        sleep 30;
        done

```

## Pre and Post Scripts

Pre and Post Scripts can be added to `DEBIAN` folder. As name suggests, they will be executed before and after the installation, carrying pre and post installation tasks.
In our example, the `preinst` scipt will archive the example.service file into a archive diretory and `postinst` script will execute `systemctl` command to enable the systemd service.

The following _preinst script_ archive the old version of systemd service.

```bash
#!/bin/bash
CURRENT=/lib/systemd/system/example.service
TARGET=/home/han/archive/example.service
if [-f "$CURRENT" ]; then
    rm $TARGET
    mv $CURRENT $TARGET
fi
```

The following _postinst script_ executes `systemctl` command to enable to systemd service.

```base
echo "your_sudo_password" | sudo -S systemctl enable example
```

## Build a Package

1. Name the content folder accoridng to the naming convention
        Debian package naming convention is:

```bash
        <project>_<major version>.<minor version>-<package revision>
```

For example, our example installation package can be named as

```bash
        installer_1.0-1
```

2. Create the folder structure according to the explanation in the previous section

```bash
example.deb
├── DEBIAN
│   ├── preinst
│   └── postinst
└── lib
    └── systemd
        └── system
              └── example.service
```

3. Add `control` file to DEBIAN folder

```bash
Package: example
Version: 1.0-1
Section: base
Priority: optional
Architecture: i386
Maintainer: Eugene Han <youremail@email.com>
Description: Hello World
 This installer installs nothing at all :)
 Yes, absolutely nothing :)))

 (the space before each line in the description is important)
```

4. Build the pacakge
Build the debian package by executing

```bash
dpkg-deb --build installer_1.0-1
```

A file named installer_1.0-1.deb will be generated. 

## Install a Package

To install the package, simply execute

```bash
sudo dpkg -i installer_1.0-1.deb
```

Restart your PC and use `sudo systemctl status example`, you should be able to see the service running.
