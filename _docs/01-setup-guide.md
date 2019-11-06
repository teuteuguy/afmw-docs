---
title: Setup Guide
layout: single
sidebar:
  nav: docs
permalink: "/docs/setup-guide/"
excerpt: How to quickly install and setup the M5StickC Amazon FreeRTOS Labs.
last_modified_at: '2019-11-06 09:53:43 +0800'
toc: true
toc_label: Contents
---

The following Setup Guide is designed to run the different labs from an [AWS Cloud9 Development ](https://aws.amazon.com/cloud9/)environment.

Once you have setup the environment, you will download the binary compiled firmware files, to flash onto your M5StickC from your own laptop.

**ProTip:** You can also follow the [`Local Setup Guide`]({{ '/docs/local-setup-guide/' | relative_url }}) to setup the environment for local development.
{: .notice--info}


## Create Cloud9 Environment

Log in to your AWS Account Console and search for Cloud9

![Start]({{ "/assets/images/setup-guide/c9-search.png" | relative_url }})

Create New Environment

![Start]({{ "/assets/images/setup-guide/c9-create-env.png" | relative_url }})

Name your environment and provide a description, then press **Next step**

![Start]({{ "/assets/images/setup-guide/c9-env-name.png" | relative_url }})

- use t2.micro instance
- set up auto-hibernate option
- press *Next step*

![Start]({{ "/assets/images/setup-guide/c9-env-settings.png" | relative_url }})

- review and press *Create environment*

Open new Terminal window

![Start]({{ "/assets/images/setup-guide/c9-new-terminal.png" | relative_url }})

## Install ESP32 Toolchain

In the Cloud9 Terminal window install prerequisites

```bash
sudo yum install flex gperf
```
```bash
sudo pip install pyserial
```

Download 64-bit version of Xtensa ESP32 toolchain:

```bash
cd ~/
wget https://dl.espressif.com/dl/xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
```

Create *esp* directory and unzip the tar archive there:

```bash
mkdir -p ~/esp
cd ~/esp
tar -xvzf ../xtensa-esp32-elf-linux64-1.22.0-80-g6c4433a-5.2.0.tar.gz
```

Add toolchain path to *~/.bash_profile* PATH variable

```bash
export PATH=$HOME/esp/xtensa-esp32-elf/bin:$PATH
```

Re-evaluate ~/bash_profile

```bash
source ~/.bash_profile
```