+++
draft = false
date = "2016-06-09T22:25:44-05:00"
title = "LXD Consul and Why I Love LXD"

+++

I have been wanting to work more closely with consul as far as developing my own daemons utilizing consul. This have given me the need for quick/on-demand development consul clusters and have found that LXD has been a great tool for me. In about 20-30 seconds, this script will give you a fully functional consul cluster with isolated and persisted data within the container much like how consul would run in a data center/cloud. 

[lxd-consul](https://github.com/mar-io/lxd-consul) is a bash script that will spin up a consul cluster on Ubuntu 16.04 LTS utilizing lxd containers.

lxd-consul has only been tested on Ubuntu 16.04 and should be used for dev/testing purposes. I have shared this for others interested in working or learning more about consul or lxd.

#### Prerequisites
* Ubuntu 16.04 LTS

* LXD install -- [howto](https://linuxcontainers.org/lxd/getting-started-cli/)

#### Installing lxd-consul

Clone the repo:
`git clone git@github.com:mar-io/lxd-consul.git`

Change Directory:
`cd lxd-consul`

Run the script:
`./lxd-consul.sh create`

At this point, the script will run, create containers, and return the IPs of the cluster.

To **stop** the cluster:

`./lxd-consul.sh stop`

To **start** the cluster:

`./lxd-consul.sh start`

To **destroy** the cluster:

`./lxd-consul.sh destroy`

#### Why LXD instead of Docker?

LXD is a very easy to use/install virtualization solution for Ubuntu users. With LXD you get all the benefits of Docker like speed, portability, isolation, and performance. However, you gain a more familiar hypervisor experience as opposed to the somewhat opinionated Docker workflow. LXD is designed to run containers which are running full operating systems which allow you to treat the container like a vm. LXD containers are smaller than traditional VM images and since they are containers they run very close to the metal. For this script, I utilized Alpine Linux images which are extraordinarily minimalist images using about 3MB of disk. Only potential downside is that LXD doesn't have a declarative configuration file (Dockerfile) or as great a mindshare/ecosystem as Docker.

I highly recommend reading about the ease and benefits of LXD here:
[LXD Tutorial](http://insights.ubuntu.com/2016/03/14/the-lxd-2-0-story-prologue/)
