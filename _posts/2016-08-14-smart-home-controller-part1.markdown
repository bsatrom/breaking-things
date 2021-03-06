---
title: Building Your Own Smart Home Controller - Part 1, The Repository
date: 2016-08-14 13:35:00 -05:00
permalink: "/posts/smart-home-controller-part1"
categories:
- smarthome
tags:
- smart-home
- home-controller
- BYO
- particle
- software
- hardware
- samsung-artik
layout: post-sidebar
comments: true
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

Over the last few months, I've spent many, many, many hours getting my gateway and repository set-up with the right software and some basic flows. My hope is that these instructions save you some time if you decide to undertake this for yourself.

This post does duplicate much of the instructions I've provided in the [Rosie-Repository repo](https://github.com/rosie-home/rosie-repository), though I wil provide a bit more context and explanation for those of you who like a bit more prose with your software setup instructions.

This is a longer, but skimmable post. I've even included a handy TOC for the jump-arounders among you:

1. [Rosie Architecture](#rosie-architecture)
2. [Role of the Repository](#role-of-the-repository)
3. [Installing InfluxDB](#installing-influxdb)
4. [Installing Telegraf](#installing-telegraf)
5. [Installing Grafana](#installing-grafana)
6. [Auto-Starting Services](#auto-starting-services)

## Rosie Architecture

The Rosie system, as I'm calling it now, consists of two major pieces of hardware in your home:

1.  A Gateway, which facilitates all messages from and communication between connected devices
2.  A Repository, which handles data storage, events and vizualizations

## Role of the Repository

The Repository is, as it sounds, the storage system for Rosie. It hosts the system databse, manages events and notifications, and provides dashboards of home data.

On the hardware side, the choice of hardware is largely up to you, though I recommend using a Raspberry Pi 3 with at least a 64GB Class 10 SD card. 

On the software side, the Rosie Repository relies on three primary software packages to do its work:

- [InfluxDB](https://influxdata.com/time-series-platform/influxdb/), the primary database for the system
- [Telegraf](https://influxdata.com/time-series-platform/telegraf/), which collects metrics about my repository and saves those to InfluxDB 
- [Grafana](https://influxdata.com/time-series-platform/telegraf/), which hosts realtime dashboards for home data hosted in InfluxDB

The instructions below assume you're installing this software on a Raspberry Pi 3 running Debian. If your hardware or OS is different, be sure to inspect the instructions below to note OS differences, which would likely manifest either in the package manager used or the platform architecture (ARM is assumed below).

## Installing InfluxDB

One of the biggest challenges I had getting my repository up and running was getting InfluxDB and Telegraf installed on the system. I tried using the prepackaged binaries provided on the InfluxDB website, but 
these didn't work for me, so I ended up building from source myself, which took a bit of doing. Hopefully these instructions will save you some time if you choose to do the same. 

You'll need to start by updating some fo the default software.

### Install Node.js and NPM

Raspbian already has node 0.10.29, but not npm. You can save yourself some headaches by 
grabbing a newer version of both.

1. Start with fetching the latest package list from your manager.

        sudo apt-get update
        
2. Install some essential build tools we need, then node and npm

        sudo apt-get install build-essential
        sudo apt-get install nodejs
        sudo apt-get install npm
        
3. Now create a symlink to your new version of node

        sudo ln -s /usr/bin/nodejs/usr/bin/node

### Install Go

To build InfluxDB (as well as Telegraf and Grafana) from source, you will need [Go](https://golang.org/).

1. Start by installing bison and the Go Version Manager (gvm)

        sudo apt-get install bison
        wget https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer
        chmod +x gvm-installer
        ./gvm-installer

  Versions of Go installed will be saved to `~/.gvm/scripts/gvm`.

2. You'll want Go 1.6, but due to a bug in the 1.6 installer, will need to install version 1.4.3 first.

        gvm install go1.4.3
        gvm use go1.4.3
        export GOROOT_BOOTSTRAP=$GOROOT
        
3. Once you have 1.4.3, install Go 1.6.2

        gvm install go1.6.2
        gvm use go1.6.2 --default
        
### Install InfluxDB

Prerequisites done, it's now time for the good stuff. Almost... 

1. Install a couple of additional dependencies before grabbing the Go source.

        sudo apt-get install ruby-dev
        sudo gem install fpm

2. Download and build InfluxDB from source

        go get github.com/influxdata/influxdb
        cd $GOPATH/src/github.com/influxdata/influxdb/
        ./build.py --package --version=0.13.0 --arch=armhf

  Setting the `arch` flag to `armhf` is critical for installing InfluxDB on ARM devices like the Pi.

  When you execute the Python build script, you might get a (non-fatal) error message. You can ignore this.

3. Finally, install InfluxDB. Note that the exact name of your `deb` package might differ from the one below. check the `build` directory to get the exact name.

        sudo dpkg -i build/influxdb_0.13.0~4254ad3_armhf.deb
  
4. To see if things worked, run the `influxd` command and then `influx`. You should jump into the InfluxDB cli once the latter command is issued.

![InfluxDB Running via a terminal](../img/post-images/influx.png)

Now that InfluxDB is up, lets jump to Telegraf...

## Installing Telegraf

Telegraf is a utility that can automatically capture and stream messages into InfluxDB (and other systems), which we're using as the data storage system on the Repository.

I'm running Telegraf on both my Gateway and Repository to collect server metrics (disk, diskio, memory, etc) about each system and stream them into InfluxDB. This allows me to monitor the brain of my smart home, and even include it in Node RED based workflows for managing my setup.

As an ARM-based system, the installation process for Telegraf on the Pi is a bit different than other Debian-based systems.

Assuming you installed InfluxDB first, you can jump right ahead to pulling the source down building it with Go. 
If you've not yet installed Go, walk through those instructions in the InfluxDB section first.

1. Grab the telegraf source

        go get github.com/influxdata/telegraf
        cd $GOPATH/src/github.com/influxdata/telegraf

2. Run the `make` command and watch the magic happen

That was easy right? No idea why Influx isn't as simple, considering both are from the folks at InfluxData.

## Installing Grafana

Now that we have InfluxDB and Telegraf set-up, let's get Grafana installed for some world-class dashboards!

1. Start by making sure that you have Node 4 or later installed.

        wget https://nodejs.org/dist/v4.0.0/node-v4.0.0-linux-armv7l.tar.gz
        tar -xvf node-v4.0.0-linux-armv7l.tar.gz
        cd node-v4.0.0-linux-armv7l
        cp -R * /usr/local

    As with before, if you've already installed InfluxDB and Telegraf, you should have Go and can grab the source. 
    If you've not yet installed Go, walk through those instructions in the InfluxDB section first.

2. Grab the Grafana source

        go get github.com/grafana/grafana
        cd $GOPATH/src/github.com/grafana/grafana

3. Run the setup script

        go run build.go setup
  
4. Grafana uses node and has a ton of dependencies. Let's install those now:

        sudo npm install node-gyp
        sudo npm install -f
        
    Make sure to use the `-f` flag to bypass issues with installing PhantomJS on ARM-based systems.

5. Install Grunt

        sudo npm install -g grunt-cli
  
6. To bypass an [issue with PhantomJS](https://github.com/grafana/grafana/issues/2683) (again), you'll need to make a change to the `build.go` file

        sudo vi build.go
  
        Go to line 76 (`:76`) and modify the line to include a `force` flag

        grunt("--force", release);
  
7. Run the build script again. There will be warnings, but it should complete and place a `.deb` file in the `dist` folder

        go run build.go build pkg-deb
  
8. Now install the package. Note that the exact name of your Debian package might be different from mine:

        sudo dpkg -i dist/grafana_4.0.0-1468683150pre1_armhf.deb

Open a browser and navigate to port `3000` on your Pi and you should see the Garfana login screen.

![Grafana Login](../img/post-images/grafana.png)
  
## Auto-starting services

Once you've installed everything, you'll no-doubt want to set up your services to auto-start on a reboot. 
This will save you time each time you power-cycle your devices. 
What's more, having your services managed by `systemd` will allow you to restart and shut them down without jumping through hoops.

### Auto-starting InfluxDB

After you install InfluxDB, you should have a file called `influxdb.service` in the `/etc/systemd/system` folder on your Pi. 

If this file *does* exist, you simply need to modify the `User` and `Group` settings on line 9 and 10 to the appropriate user and group (aka the user under which you installed InfluxDB).

1. If this file *doesn't* exist, you'll need to create it:

        cd /etc/systemd/system/
        touch influxdb.service
        vi influxdb.service
        
2. Then copy the service file from the [InfluxDB repository here](https://github.com/influxdata/influxdb/blob/master/scripts/influxdb.service).

3. Modify the `User` and `Group` settings on line 9 and 10 to the appropriate user and group (aka the user under which you installed InfluxDB).

4. Now reload the systemd daemon and enable InfluxDB:

        sudo systemctl daemon-reload
        sudo systemctl enable influxdb
        sudo systemctl start influxdb

### Auto-starting Telegraf

Unlike InfluxDB, Telegraf didn't put a `service` file in my `/etc/systemd/system` folder, so I added one myself:

1. Crete the file

        cd /etc/systemd/system
        touch telegraf.service
        vi telegraf.service
        
2. Copy the service file from the [Telegraf repository](https://github.com/influxdata/telegraf/blob/master/scripts/telegraf.service).

3. Change the `user` setting on line 8 to the appropriate user.

4. Determine where telegraf is installed on your system and copy the result.

        which telegraf
  
    For my installation, it was `/home/pi/.gvm/pkgsets/go1.6.2/global/bin/telegraf`

5. Modify the path to the `telegraf` command on line 11 to the correct location of your telegraf installer.

6. Create a `telegraf` directory in `etc/default`

        sudo touch /etc/default/telegraf
  
7. Reload the systemd daemon and enable InfluxDB

        sudo systemctl daemon-reload
        sudo systemctl enable telegraf
        sudo systemctl start telegraf
        
### Auto-starting Grafana

Auto-starting Grafana is the simpliest of all

        sudo systemctl daemon-reload
        sudo systemctl enable grafana-server
        sudo sysatemctl start grafana-server

And you're done. You Repository and Gateway are ready for you to use to automate your home! In a future post, I'll help you get started with node-red, setup dashboards and more!