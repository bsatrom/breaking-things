---
title: Building Your Own Smart Home Controller - Part 2, The Gateway
date: 2016-08-02 13:35:00 -05:00
permalink: "/posts/smart-home-controller-part2"
categories:
- smart-home
tags:
- smart-home
- home-controller
- BYO
- particle
- software
- hardware
- samsung-artik
layout: post-sidebar
author_name: Brandon Satrom
author_url: "/author/brandon"
author_avatar: brandon
show_avatar: true
read_time: 15
feature_image: feature-blueprint
show_related_posts: true
square_related: recommend-blueprint
---

Several months ago, I decided to build my own smart home controller. My reasons are many, and I'll detail a few in a future post or two, but suffice to say that I wanted complete control over the devices in my home, and do not want to bring in an appliance rife with DRM that may one day turn into an expensive, dead plastic box.

I'm at the beginning of my journey, but my aspiration is to build something that others can also use and build upon, which is why I and a few others started the [Rosie project](https://github.com/rosie-home) on GitHub. More on Rosie in a future post as well.

Over the last few months, I've spent many, many, many hours getting my gateway and hub set-up with the right software and some basic flows. My hope is that these instructions save you some time if you decide to undertake this for yourself.

This post does duplicate much of the instructions I've provided in the [Rosie-Gateway repo](https://github.com/rosie-home/rosie-gateway), though I wil provide a bit more context and explanation for those of you who like a bit more prose with your software setup instructions.

This is a longer, but skimmable post. I've even included a handy TOC for the jump-arounders among you:

1. [Rosie Architecture](#rosie-architecture)
2. [Role of the Gateway](#role-of-the-gateway)
3. [Installing Node RED](#installing-node-red)
4. [Installing Mosquitto](#installing-mosquitto)
5. [Installing Telegraf](#installing-telegraf)
6. [Auto-Starting Services](#auto-starting-services)

## Rosie Architecture

The Rosie system, as I'm calling it now, consists of two major pieces of hardware in your home:

1.  A Gateway, which facilitates all messages from and communication between connected devices
2.  A Repository, which handles data storage, events and vizualizations

## Role of the Gateway

The Gateway is the brain of the home and its central to its function. As much as possible in my home, I want to minimize direct chatter between devices (and chatter to the cloud, but this is not always possible with 3rd party smart devices) and ensure that the gateway orchestrates everything in my home.

On the hardware side, the choice of hardware is largely up to you, though I recommend a device that supports a variety of communication protocols to ensure that you can support a broad number of devices. Alternatively, you can grab a Pi 2 or Pi 3 and add hats or your own add-ons to expand support, but for my initial prototypes I decided to use the [Samsung Artik 5](http://artik.io) which supports WiFi, BLE, ZigBee, ZWave, Thread and SigFox.

On the software side, the Rosie Gateway relies on three software packages to do its work:

- [Node-RED](http://nodered.org/)
- [Mosquitto](https://mosquitto.org/)
- [Telegraf](https://influxdata.com/time-series-platform/telegraf/)

All of the packages below were installed on a Samsung Artik 5, which runs Fedora 22. If you're using a Raspberry Pi, the instructions will be similar, but you'll use `apt-get` for installing packages as opposed to `dnf`. If you're using another board or computer, modify the instructions below to fit your OS's package manager.

## Installing Node RED

At the moment, Rosie relies on Node RED as the development tool and engine for workflows around my smart home. This tool, which was incubated in the IBM Emerging Tech group, is a great way to construct and visualize interactions with and messages between devices. There's a lot of out of the box functionality, and a ton of plugins (distrubted via npm) and sample flows contributed by the community.

I have my gripes with Node RED, and I expect we'll either fork it or replace it with something else moving forward, but its been a great tool for getting started with our smart home MVP.

Also, getting it up and running is simple...

1. Update your package manager (dnf in the case of Fedora) to ensure you have the last package list
 
   ```bash
   $ dnf update
   ```
  
  You might find some instructions online that tell you to use `yum` on Fedora. `yum` has been deprecated, so while still works on Fedora 22, it redirects to `dnf` automatically.

2. Install node & npm

   ```bash
   $ dnf install node
   $ dnf install npm
   ```

3. Install node-red
  
   ```bash
   $ npm install -g node-red
   ```

4. 	Start node-red (as a background process) and make sure everything runs fine

    ```bash
    $ node-red &
    ```

If everything works, your terminal should display a message with the ip address and port (`1833` is the default) Node RED is running on. Open a browser window and behold the new brain of your smart home.

## Instaling Mosquitto

Mosquitto is an open source utility that facilitates publishing and subscribing to MQTT messages. MQTT messages are common in IoT environments because the payload is small and the pub-sub API is easy to configure. Node RED supports MQTT out of the box, and I'll be using it for all of the custom sensors in my house.

Assuming you've already installed node-red as listed above, installing mosquitto is simple

```bash
$ dnf install mosquitto
```

Now, run mosquitto as a background process to make sure that things are ok.

```
$ mosquitto &
```

If everything works, you should get an ip address and port for the MQTT server running on your gateway. To test it out, you can set up a subscription and send a sample message:

```
mosquito_sub –d –t msq_test
```

Now, open up a separate tab in the same directory and send a message:

```
mosquito_pub –d –t mqtt_test –m “Hello Rosie"
```

If everything works, you should see a "Hello Rosie" message in the terminal window where you subscbived to messages.

## Installing Telegraf

The final piece of software I'm running on my gateway is an optional one, but important nevertheless. It's a tool called Telegraf from the folks at InfluxData. Telegraf is a utility that can automatically capture and stream messages into InfluxDB (and other systems), which I'm as the data storage system on my Repository.

I'm running Telegraf on both my Gateway and Repository to collect server metrics (disk, diskio, memory, etc) about each system and stream them into InfluxDB. This allows me to monitor the brain of my smart home, and even include it in Node RED based workflows for managing my setup.

As an ARM-based system, the installation process for Telegraf on the Artik is a bit different than other Fedora-based (RedHat or CentOS) devices.

To collect server stats from the gateway, you'll need to install telegraf.

```bash
cd ~
wget https://dl.influxdata.com/telegraf/releases/telegraf-0.13.1-1_linux_armhf.tar.gz
tar xvfz telegraf-0.13.1-1_linux_armhf.tar.gz
cd telegraf-0.13.1-1
```

Once you've installed Telegraf, you'll need to modify the `telegraf.conf` file to log to the remote InfluxDB instance on your Repository. 

**Note**: If you've not yet completed the [Repository setup steps](https://github.com/rosie-home/rosie-repository/blob/master/docs/softwaresetup.md) do that first and then return to complete configuring Telegraf on your gateway.

Open the `telegraf.conf` file for editing

```
vi etc/telegraf/telegraf.conf
```

On line 60 (`:60`), set the `hostname` to your gateway named

```bash
hostname = "Rosie-Gateway"
````

On line 74 (`:74`), set the `urls` value to the IP address of your repository server

```bash
urls = ["http://10.171.5.129:8086"]
```

On line 76 (`:76`), set the database value to the same database you're using for repository statistics (default is `telegraf`)

```bash
database = "telegraf"
```

On line 82 (`:82`), set the `retention_policy` value. 

Every InfluxDB database has one or more rentention policies that dictate how long data is stored before deletion. On my repository, I have a few policies, one that keeps data for two days and one for two weeks. Since I only want server data for heartbeat purposes, and it logs frequently, I'll use a shorter policy.

For more information on creating retention policies in InfluxDB [see this article](https://docs.influxdata.com/influxdb/v0.13/query_language/database_management/#retention-policy-management).

```bash
## Retention policy to write to.
retention_policy = "two_days_only"
```

On lines 89 and 90, set the user name, set the username and password for your InfluxDB instance

```
username = "admin"
password = "admin"
```

Hit the `ESC` key and type `wq!` to save your changes.

To ensure that everything is working, run telegraf with the recently edited config file:

```
~/telegraf-0.13.1-1/usr/bin/telegraf -config ~/telegraf-0.13.1-1/etc/telegraf/telegraf.conf
```

## Auto-starting services

Once you've installed node-red, mosquitto and Telegraf on your Gateway, you'll want to configure them to auto-run when you hub starts up.

#### Auto-starting node-red

First, create a new file named `nodered` at /etc/init.d/ and change permissions on the file to enable execution

```
$ mkdir /etc/init.d/nodered
$ chmod 755 /etc/init.d/nodered
```

Download [this init.d script](https://gist.github.com/bigmonkeyboy/9962293) for node-red and copy it into the file you created in the last step. Alternatively, you can copy the code below. If you download the file linked, be sure to 
change the first uncommented line from `USER=pi` to `USER=root`.

```bash
#! /bin/sh
# Starts and stops Node-RED
# /etc/init.d/nodered
### BEGIN INIT INFO
# Provides:     node-red
# Required-Start:       $syslog
# Required-Stop:        $syslog
# Default-Start:        2 3 4 5
# Default-Stop:         0 1 6
# Short-Description:    Node-RED initialisation
### END INIT INFO
# Can be downloaded and installed in one go by using this command
# sudo wget -O /tmp/download https://gist.github.com/bigmonkeyboy/9962293/download && sudo tar -zxf /tmp/download --strip-components 1 -C /etc/init.d && sudo chmod 755 /etc/init.d/nodered && sudo update-rc.d nodered defaults

# This runs as the user called pi - please change as you require
USER=root

# The log is written to here - please make sure your user has write permissions.
LOG=/var/log/node-red.log

#Load up node red when called
case "$1" in

start)
    if pgrep ^node-red$ > /dev/null
    then
        echo "Node-RED already running."
    else
        echo "Starting Node-Red.."
        touch $LOG
        chown $USER:$USER $LOG
        echo "" >> $LOG
        echo "Node-RED service start: "$(date) >> $LOG
#        su -l $USER -c "cd ~/.node-red && screen -dmS red node-red-pi --max-old-space-size=128"
# or
        su -l $USER -c "node-red-pi --max-old-space-size=128 -u ~/.node-red >> $LOG &"
        echo "Logging to "$LOG
    fi
;;

stop)
    echo "Stopping Node-Red.."
#        su -l $USER -c "screen -S red -X quit"
# or
    pkill -SIGINT ^node-red$
    sleep 2
    echo "" >> $LOG
    echo "Node-RED service stop: "$(date) >> $LOG
;;

restart)
        echo "Restarting Node-Red.."
        $0 stop
        sleep 2
        $0 start
        echo "Restarted."
;;
*)
        echo "Usage: $0 {start|stop|restart}"
        exit 1
esac
```

Use `chkconfig` to modify runlevel settings on the nodered service

```
$ chkconfig --add nodered
$ chkconfig --level 2345 nodeted on
```

Start the service to make sure everything worked

```
/etc/rc.d/init.d/nodered start
```

#### Auto-starting mosquitto

Create a conf file basd on the preinstalled example file

```
mv /etc/mosquitto/mosquitto.conf.example /etc/mosquitto/mosquitto.conf
```

Use the systemd utility to enable and start the service

```
systemctl enable mosquitto.service
systemctl start mosquitto.service
```

#### Auto-starting Telegraf

Create a service file in the Systemd folder. 

```
touch /usr/lib/systemd/system/telegraf.service
```

If the file already exists, skip to the next step.

Open the file for editing and paste the following

```
[Unit]
Description=The plugin-driven server agent for reporting metrics into InfluxDB
Documentation=https://github.com/influxdata/telegraf
After=network.target

[Service]
EnvironmentFile=-/etc/default/telegraf
User=root
Environment='STDOUT=/var/log/telegraf/telegraf.log'
Environment='STDERR=/var/log/telegraf/telegraf.log'
ExecStart=/bin/sh -c "exec ~/telegraf-0.13.1-1/usr/bin/telegraf -config ~/telegraf-0.13.1-1/etc/telegraf/telegraf.conf ${TELEGRAF_OPTS} >>${STDOUT} 2>>${STDERR}"
ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
KillMode=control-group

[Install]
WantedBy=multi-user.target
Alias=telegraf.service
```

Finally, reload the systemd daemon and start the service

```
systemctl daemon-reload
systemctl enable telegraf
systemctl start telegraf
```

And that's that! You now have a smart home gateway, ready to facilitate all of your futuristic interactions!

