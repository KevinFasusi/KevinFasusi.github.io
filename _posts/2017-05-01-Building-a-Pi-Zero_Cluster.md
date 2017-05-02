---
layout: posts
excerpt_separator: <!--more -->
title: Building a Pi Zero Cluster using a ClusterHat
comments: False
---

{% include header.html %}
I wanted to explore distributed computing and also experiment with Docker swarm. After looking into building a raspberry pi cluster, I found something called a ClusterHat sold by [Pimoroni](https://shop.pimoroni.com/products/cluster-hat). <!--more --> It allows you to connect the Pi zeros together over an OTG Network, without the hassle of ethernet wiring and a switch. I already had two Pi zeros, so I bought another two pi zeros and a ClusterHat. Look at this bad boy:

![pi zero cluster with clusterhat]({{base}}/assets/clusterhat.png)


# Setting up the Cluster

The company responsible for the ClusterHat ([clusterhat](https://clusterhat.com/)) provide documentation for the assembly, software and control. Broadly speaking, the process involves:

- Downloading five different iso images and mounting the images to micro sd card. Use [Etcher](https://etcher.io/) for burning the image.

- Resizing the partition on the micro sd card.

- Changing the default password.

- adding ssh file to the boot folder (if you plan to ssh into the controller and pi zeros). 

The ability to ssh is disabled in the recent versions of Rasbian operating system for security reasons. The easiest way to enable ssh again is to place the micro sd card into an SD card adapter and create the file in the boot folder from another machine (laptop). Then return the micro sd card back to the Pi zero.

# Setting up Docker

Once the nodes are all setup, I installed Docker on each one using `curl -sSL https://get.docker.com | sh`. The cli will prompt you to changer the docker privileges using `sudo usermod -aG docker pi`. Initiate docker swarm on the controller pi `docker swarm init`.

After setting up the manager node, the cli will output a command to run on each node (pi zeros) to join the swarm. 

![swarm]({{base}}/assets/swarm.png)

Now I have a cluster running docker, and that's nice.