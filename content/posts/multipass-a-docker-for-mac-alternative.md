+++
draft = false
date = "2021-11-12T22:25:44-05:00"
title = "Multipass a Docker Desktop for Mac alternative"

+++

# Intro 

[Multipass](https://multipass.run) is described as a mini cloud on your computer. It's a pretty elegant tool based on the great design of [LXD](https://linuxcontainers.org/lxd/introduction/), another tool from Canonical that I really enjoy. I prefer using Multipass for my local docker host needs over Docker Desktop For Mac. Part of this based on my experience as a dev tooling Engineering Manager. Docker Desktop For Mac always seemed to be slow and bulky and this can cause some issues when rolling out as a dev tool at scale. Multipass in my experience, thus far, seems much less resource intensive. Furthermore, coupled with [cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/examples.html) you can use declarative files to bootstrap a multipass server.

This guide will walk you through installing Multipass and configuring a Docker server to use with your Mac's docker client. Essentially, a lightweight alternative to Docker Desktop For Mac.

# Tutorial

## Install Multipass and Docker Client 

`brew install multipass docker`


## Create a cloud-init yaml

Create and save cloud-init yaml to a desired location:

`vim $HOME/docker4mac.yaml`

Copy the below yaml:

```
---
users:
  - name: ubuntu
    sudo: ALL=(ALL) NOPASSWD:ALL
package_update: true
write_files:
- content: |
    {"hosts": ["tcp://0.0.0.0:2375", "unix:///var/run/docker.sock"]}
  path: /etc/docker/daemon.json
- content: |
    [Service]
    ExecStart=
    ExecStart=/usr/bin/dockerd
  path: /etc/systemd/system/docker.service.d/override.conf
packages:
  - docker
  - avahi-daemon
  - apt-transport-https
  - ca-certificates
  - curl
  - gnupg
  - lsb-release
runcmd:
  - sudo curl -fsSL https://get.docker.com | sudo bash
  - sudo systemctl enable docker
  - sudo groupadd docker
  - sudo usermod -aG docker ubuntu
  - sudo systemctl daemon-reload
  - systemctl restart docker.service
```


## Create a docker host in multipass

`multipass launch -c 2 -m 1G -d 25G -n docker lts --cloud-init $HOME/docker4mac.yaml`


## Add DOCKER_HOST environment variable to .zshrc

`echo "export DOCKER_HOST=tcp://$(multipass info docker | grep IPv4 | awk '{print $2}'):2375" >> $HOME/.zshrc`

At this point you should have docker ready to go on your Mac. Test it out!

`docker pull alpine`

If that works you are all set.


# Conclusion

So my experience has been great and snappier than my less than fond memories of using Docker Desktop For Mac. Granted, I am using the latest greatest [Apple M1 Chip](https://www.apple.com/newsroom/2020/11/apple-unleashes-m1/) and this baby purrs. 


# Other Considerations

I looked at Podman but its Mac implementation is experimental. The [Red Hat](https://www.redhat.com/sysadmin/replace-docker-podman-macos) solution requires using ssh to connect to the remote Podman server. All in all, pretty unelegant.  

Happy Containerizing!
