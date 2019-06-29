---
title: Systemd Basics
date: 2019-06-07 01:57:37
tags: Linux, Systemd
---

Systemd is a Linux system tool which can be used to start, schedule, monitor, manage system processes and customer applications. I used systemd while configuring my minimal Debian application and run some application upon OS bootup and manage the lifecycle of applications.

This blog can guides you through some of the basics of systemd services. 

## Create a Custom systemd Service (Use HelloWorld as an example)

1. Create a script or executable that the service will manage.

```bash
        #!/bin/bash
        #Path: /home/han/test_service.bash
        /usr/bin/java -jar /home/structo/HelloWorld.jar
        while :
        do
        echo "Looping...";
        sleep 30;
        done
```

2. Make it executable.

```bash
        [sudo] chmod +x /home/structo/test_service.sh
```

3. Create Service file.

```bash
        #Path: /lib/systemd/system/myservice.service
        [Unit]
        Description=Example systemd service.

        [Service]
        Type=simple
        ExecStart=/bin/bash /home/structo/test_service.sh
        User=structo

        [Install]
        WantedBy=multi-user.target
```

4. Start and enable the service.

```bash
        # reload systemd service after editing
        [sudo] systemctl daemon-reload

        # start service immediately
        [sudo] systemctl start myservice

        # read the status and partial log of myservice
        [sudo] systemctl status myservice

        # stop myservice
        [sudo] systemctl stop myservice

        # restart myservice
        [sudo] systemctl restart myservice

        # my executing enable command, a symblic link will be formed to 
        multi-user.target.wants
        # which means the service will be started every time system reboots
        [sudo] systemctl enable myservice
        Created symlink from /etc/systemd/system/multi-user.target.wants/
        myservice.service to /lib/systemd/system/myservice.service.

        # to view the full log of the service
        [sudo] journalctl -u myservice.service
```
