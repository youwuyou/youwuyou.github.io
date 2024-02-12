---
layout: post
title: Using Plastic SCM on Arch Linux
date: 2024-02-11 13:04:00
description: This post describes how to use distrobox to run containerised applications
tags: OS⚙️
categories: Coding💻
giscus_comments: true
featured: false
toc:
  sidebar: left
---

From January 26th to 28th, I participated in the Global Game Jam 2024 at ETH Zurich, where I was part of a team of five. We worked on a game around the theme "Make Me Laugh" for 48 hours and had the game [CArT Lander](../../../projects/game_cart_lander/) as our final product. We decided to use [Plastic SCM](https://www.plasticscm.com/) for version control. Unfortunately, it is not available on my system, Garuda Linux. But the tool `distrobox` turned out to be the a perfect solution that resolved the problem.

## distrobox

The [distrobox](https://wiki.archlinux.org/title/Distrobox) resides in the [extra repository](https://wiki.archlinux.org/title/Official_repositories#extra) and can be downloaded using pacman with the command `sudo pacman -S distrobox`.

Create a container with desired configuration

```bash
# Create a container using distrobox-create --name container-name --image os-image:version
distrobox-create --name ubuntu-20 --image ubuntu:20.04

# Enter the container
distrobox-enter ubuntu-20

# Check container status
distrobox-list
```

Once we can successfully access the shell of the newly created container, we just need to follow the [installation instructions](https://www.plasticscm.com/plastic-for-linux) of your chosen linux distribution as on official Plastic SCM website.

```bash
# 1. ADD THE PLASTIC SCM REPOSITORY
sudo apt-get update
sudo apt-get install -y apt-transport-https
echo "deb https://www.plasticscm.com/plasticrepo/stable/ubuntu/ ./" | sudo tee /etc/apt/sources.list.d/plasticscm-stable.list
wget https://www.plasticscm.com/plasticrepo/stable/ubuntu/Release.key -O - | sudo apt-key add -
sudo apt-get update

# 2. INSTALL PLASTIC SCM (default installation)
sudo apt-get install plasticscm-complete
```

Once the installation is done, check that the plastic scm is working with some commands like `cm --help`. The last step is to export the application from the container to our host.

```bash
# Run inside of the container, and integrate the application with our desktop.
distrobox-export --app cm
```

## Summary

With distrobox, one can containerize a desired application that is not avaliable for current platform. For lightweight applications such as Plastic SCM, this approach works out smoothly and is highly recommended.