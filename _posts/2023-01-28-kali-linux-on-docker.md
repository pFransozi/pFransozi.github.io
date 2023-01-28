---
layout: post
title:  kali linux on docker
date:   2023-01-28 01:00:00
description: some steps to configure docker and kali linux on docker
language: en-us
tags: ["docker", "kali-linux"]
language: en-us
category: ["docker", "containerization"]
---
## Introduction

This tutorial aims to create a kali linux environment that runs in a docker container. There're two main steps to reach it:

1. Install docker;
2. Configure a dockerfile to create a container based on kali linux.

I consider some kali-meta packages, some for the linux environment others tools for the ethical hacking:

* kali-linux-default;
* kali-tools-wireless;
* kali-tools-web;
* kali-tools-forensics;
* kali-tools-exploitation;
* kali-tools-database;
* kali-tools-crypto-stego;
* kali-tools-passwords.

So far, I consider access kali only through command line interface, but, for the future, I consider configure a graphic user interface access, probably through VNC or RDP. This is a testing phase, and my reasons why I chose this environment are based on the advantages of containerization over virtualization, such as:

* more efficient resource utilization;
* faster deployment and scaling
* and easier management and maintenance of applications

## Hands-on

### Install Docker Engine

**First step**, as best practices recommend, let's update the package lists and remove any previous version of docker that is installed in the OS, which includes installed packages, configuration files, and images, containers and volumes. If you sure that it is the first installation, skip to second step.

~~~ shell
sudo apt update

## list packages related to docker
dpkg -l | grep -i docker

## based on the packages found, remove them 
## and the configuration files related to them, using apt purge
sudo apt purge  docker \
                docker-ce \
                docker-ce-cli \
                docker-clean \
                docker-compose \
                docker-doc \
                docker-registry \
                docker.io \
                docker2aci 
## and then removes docker engine and related packages and their dependencies from
sudo apt-get autoremove -y --purge  docker-engine \
                                    docker \
                                    docker.io \
                                    docker-ce \
                                    docker-compose-plugin

## after that, remove images, containers, and volume:
sudo rm -rf /var/lib/docker /etc/docker
sudo rm /etc/apparmor.d/docker
sudo groupdel docker
sudo rm -rf /var/run/docker.sock
~~~

**Second step**, let's start installation from packages to allow apt to use a repository over HTTPS

~~~ shell
## curl provides a CLI tool for perform HTTP-related tasks and others protocols
## ca-certificates contains a set of common root certificates that are used to stablish 
## trust when connecting to SSL/TLS-protected websites.
## gnupg provides an OpenPGP standard for encrypting and signing data. It allows you to 
## securely exchange data with other users, and to verify the authenticity of signed data.
## lsb-release provides lsb_release command, which is used to display information about
## the Linux Standard Base (LSB) release and distribution of the current system.
sudo apt install ca-certificates curl gnupg lsb-release

## docker.gpg is the file used to verify the authenticity of a docker package.
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

## configure the APT package manager to use the Docker package repository for installing 
## Docker-related software on Ubuntu.
echo \
  "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null


## then update package lists and install docker packages.
sudo apt update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin -y
~~~

After that, you can run docker using the next command: `docker run hello-word`. But probably you'll get a permission denied error. Let's understand it more closely. 

The steps above install docker in a default way, and by default means that docker requires root privileges because it needs access to resources that are normally restricted to root-level processes. Those resources are critical to functioning of the OS, and for that allowing non-root processes to access them could potentially compromise the system's security and stability. Docker bypasses that running a process called Docker Engine daemon in background, and usually it starts at boot time, with root privileges for managing containers. By default, It listens for API requests on a Unix socket located at `/var/run/docker.sock`.

Reading something here and there, I think running Docker as root is generally less secure than running it as a non-root user. This is because the root user has full privileges on the system, which means that any Docker vulnerabilities could potentially be exploited by an attacker to gain full access to the host machine.

If you don't worry, you can use docker cli as root privileges, always that you need to interact with docker daemon, as next: `sudo docker run hello-world`.

In docs.docker.com, there're two ways for that situation, one is a really solution, other is a work-around, which means the daemon still run in privilege mode, but the docker command does not need be prefaced by sudo anymore. The work-around is to add an user into docker group, granting to it privilege to access `/var/run/docker.sock`.

It's important to understand that it does not change the fact that daemon docker is running as root privileges. It just means that you do not need preface docker command with sudo. 

~~~ shell
## create a new group called docker, sometimes package installer already created it.
sudo groupadd docker

## modify user account, appending $USER into a group.
## replace $USER for a real user.
sudo usermod -aG docker $USER

## reboot the OS, I always needed do that.

## update the user to the new group in the current shell session. 
newgrp docker
## then,
docker run hello-world
~~~

### Configure a Dockerfile

A dockerfile is a text file that contains a script for building a docker image. The main purpose of using a dockerfile is to automate the process of building a docker image because in it there are all of the steps required to create it. Thus, the result image can be consistently reproduced. 

The dockerfile starts with a reference to a base image, in which our image will be built. In our case it is kalilinux/kali-rolling, which is a suggestion from kali.org/docs. That image tracks the continuously-updated kali-rolling package repository.

~~~ dockerfile
FROM kalilinux/kali-rolling
~~~

Next, `LABEL` directives are added into the dockerfile, which allow to add metadata to a docker image. For instance:

~~~ dockerfile
LABEL docker.version=
LABEL docker.release-date=
LABEL maintainer=
~~~

Labels are optional in a dockerfile, but they can be useful for providing information about the image to users and for tracking changes to the image over time. Labels can be viewed using the docker `inspect` command, and they can also be used to filter and search for images in a registry. For view information about a container, use the next command: `docker inspect container-name`.

Return to dockerfile, some commands are executed into the base image, for that `RUN` directive is used. It's important to take care if the command has any interaction with the user, if so, the build will be interrupted. 

> It's important a non interactive mode for guarantee automation. 

For a non-interactive mode in a debian environment it is recommended to use the `DEBIAN_FRONTEND=noninteractive` environment variable. This variable is commonly used when running command-line tools or scripts that perform automated tasks, such as installing software or configuring system settings. By setting `DEBIAN_FRONTEND=noninteractive`, the user can specify that the script should run without prompting for user input or confirmation, allowing the script to run automatically without intervention.

With `DEBIAN_FRONTEND=noninteractive` and `apt install -y` any instruction that requires information will use the default value.

~~~ dockerfile
RUN DEBIAN_FRONTEND=noninteractive apt update && \
    apt install -y \
    kali-linux-default \
    kali-tools-wireless \
    kali-tools-web \
    kali-tools-forensics \
    kali-tools-exploitation \
    kali-tools-database \
    kali-tools-crypto-stego \
    kali-tools-passwords
~~~

Thus, the `RUN` directive above update the list of the package manager and then it installs some packages from kali. This is a task that takes a substantial amount of time on its first run. 

It's important understand that docker builds dockerfile using a layered approach. When building an image, docker creates a layer for each command in the Dockerfile and assigns a unique hash to each layer. The hash is based on the contents of the layer and the dependencies of the layer, and is used to identify the layer and track changes to it.

The next steps are focused on configure the environment at the user level. First it creates a directory `/mnt/shared` that will be shared with host environment. `VOLUME` directive specifies a mount point for external storage in a container. 

A volume is a directory that is stored outside the container's filesystem and can be shared among multiple containers. Volumes are useful for storing persistent data that needs to be retained across container restarts, or for sharing data among containers. 

> It's important to note that the `VOLUME` directive does not create the actual volume or allocate any storage for it. Instead, it simply creates a mount point in the container that can be used to mount a volume from the host or from another container.

~~~ dockerfile
RUN mkdir /mnt/shared
VOLUME /shared
~~~

Then, a configuration zsh script is executed, which is downloaded from zsh default repository. And it is set as default shell for the current environment using `ENV` directive and `SHELL /usr/bin/zsh`.

~~~ dockerfile
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.2/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git \
    -p ssh-agent \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions

ENV SHELL /usr/bin/zsh
~~~

`ENV` is a directive used to send environment variable in the current shell and `SHELL` is a unix env variable that is used to determine which shell to use. By default, the bash shell is often used, but you can change the default shell by setting the `SHELL` environment variable to the path of the desired shell.

Then, the next `RUN` directive executes three commands: `useradd` command adds a new user into the OS, then the password is changed using `echo "USER:PASS" | chpasswd`, and the user is added to the group `SUDO`, which allows that the user can require privileges.

~~~ dockerfile
RUN useradd -m USER && echo "USER:PASS" | chpasswd && adduser USER sudo
~~~

In last three lines of dockerfile, the `WORKDIR` directive sets the working directory for any command that follow it in the Dockerfile, and it also sets the default working directory for the container when it starts. `USER` directive sets the default user when container starts and `CMD` directive executes `zsh`.

~~~ dockerfile
USER kali
WORKDIR /home/kali
CMD ["zsh"]
~~~

## Conclusion

The entire script follows:

~~~ dockerfile
FROM kalilinux/kali-rolling

LABEL docker.version=
LABEL docker.release-date=
LABEL maintainer=

# Update the package manager and install the command line tools
RUN DEBIAN_FRONTEND=noninteractive apt-get update 
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    kali-linux-default \
    kali-tools-wireless \
    kali-tools-web \
    kali-tools-forensics \
    kali-tools-exploitation \
    kali-tools-database \
    kali-tools-crypto-stego \
    kali-tools-passwords

# Set up shared directory with host machine
RUN mkdir /mnt/shared
VOLUME /shared

# zsh configuration https://morioh.com/p/c3a94a037600
# https://github.com/ohmyzsh/ohmyzsh/wiki/Themes?ref=morioh.com&utm_source=morioh.com
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.2/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git \
    -p ssh-agent \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions

ENV SHELL /usr/bin/zsh

# Create a non-root user with sudo privileges
RUN useradd -m [USER] && echo "[USER]:[PASS]" | chpasswd && adduser [USER] sudo

USER [USER]
WORKDIR /home/[USER]
CMD ["zsh"]
~~~

To build that dockerfile, use a command such as: `docker build --pull --rm -f "Dockerfile" -t kali-docker-1 "."`
To run that image, use a command such as: `docker run --tty --interactive -v /host/dir:/shared kali-docker-1:latest`

## References

[Docker Engine installation overview](https://docs.docker.com/engine/install/){:target="_blank"}.

[Run the Docker daemon as a non-root user (Rootless mode)](https://docs.docker.com/engine/security/rootless/){:target="_blank"}.

[Docker security](https://docs.docker.com/engine/security/#docker-daemon-attack-surface){:target="_blank"}.

[How to completely uninstall docker](https://askubuntu.com/questions/935569/how-to-completely-uninstall-docker){:target="_blank"}.

[Kalilinux docker images](https://www.kali.org/docs/containers/official-kalilinux-docker-images/){:target="_blank"}.

[Using Kalilinux docker images](https://www.kali.org/docs/containers/using-kali-docker-images/){:target="_blank"}.
