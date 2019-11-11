---
title: Local Setup Guide
layout: single
sidebar:
- nav: docs
permalink: /docs/local-setup-guide
excerpt: How to setup the development environment locally.
last_modified_at: '2019-11-06 09:53:43 +0800'
toc: true
toc_label: Contents
---

The following guide will help you get setup on your own laptop to run the code for the follwing labs on the M5StickC.

**ProTip:** Most of this guide is based on the official Amazon FreeRTOS [documentation](https://docs.aws.amazon.com/freertos/latest/userguide/getting_started_espressif.html). Please refer to that for the single source of truth.
{: .notice--info}

# Set Up Your Development Environment
To communicate with your board, you need to download and install a toolchain.

# Setting Up the Toolchain
To set up the toolchain, follow the instructions for your host machine's operating system:

**Note:** When you reach the `"Get ESP-IDF"` instructions under **Next Steps**, stop and return to the instructions on this page. If you previously followed the `"Get ESP-IDF"` instructions and installed ESP-IDF, make sure that you clear the IDF_PATH environment variable from your system before continuing.
{: .notice--danger}

* [Standard Setup of Toolchain for Windows]({{ '/docs/local-setup-guide#standard-setup-of-toolchain-for-windows' | relative_url }})
* [Standard Setup of Toolchain for macOS]({{ '/docs/local-setup-guide#standard-setup-of-toolchain-for-macos' | relative_url }})

**Note:** Version 3.1.5 of the ESP-IDF (the version used by Amazon FreeRTOS) does not support the latest version of the ESP32 compiler. You must use the compiler that is compatible with version 3.1.5 of the ESP-IDF (see the links above). To check the version of your compiler, run "xtensa-esp32-elf-gcc --version".
{: .notice--info}

## Standard Setup of Toolchain for Windows

**Note:** This section is copied from the official [ESP32 Windows documentation](https://docs.espressif.com/projects/esp-idf/en/v3.1.5/get-started-cmake/windows-setup.html).
{: .notice--info}

### ESP-IDF Tools Installer

The easiest way to install ESP-IDF’s prerequisites is to download the ESP-IDF Tools installer from this URL:

[https://dl.espressif.com/dl/esp-idf-tools-setup-1.1.exe](https://dl.espressif.com/dl/esp-idf-tools-setup-1.1.exe)

The installer will automatically install the ESP32 Xtensa gcc toolchain, [Ninja](https://ninja-build.org/) build tool, and a configuration tool called [mconf-idf](https://github.com/espressif/kconfig-frontends/releases/). The installer can also download and run installers for [CMake](https://cmake.org/download/) and [Python 2.7](https://www.python.org/downloads/windows/) if these are not already installed on the computer.

By default, the installer updates the Windows `Path` environment variable so all of these tools can be run from anywhere. If you disable this option, you will need to configure the environment where you are using ESP-IDF (terminal or chosen IDE) with the correct paths.

Note that this installer is for the ESP-IDF Tools package, it doesn’t include ESP-IDF itself.

### Installing Git
The ESP-IDF tools installer does not install Git. By default, the getting started guide assumes you will be using Git on the command line. You can download and install a command line Git for Windows (along with the “Git Bash” terminal) from [Git For Windows](https://gitforwindows.org/).

If you prefer to use a different graphical Git client, then you can install one such as Github Desktop. You will need to translate the Git commands in the Getting Started guide for use with your chosen Git client.

### Using a Terminal
For the remaining Getting Started steps, we’re going to use a terminal command prompt. It doesn’t matter which command prompt you use:

* You can use the built-in Windows Command Prompt, under the Start menu. All Windows command line instructions in this documentation are “batch” commands for use with the Windows Command Prompt.
* You can use the “Git Bash” terminal which is part of [Git for Windows](https://gitforwindows.org/). This uses the same “bash” command prompt syntax as is given for Mac OS or Linux. You can find it in the Start menu once installed.
* If you have [MSYS2](https://msys2.github.io/) installed (maybe from a previous ESP-IDF version), then you can also use the MSYS terminal.

## Standard Setup of Toolchain for macOS

**Note:** This section is copied from the official [ESP32 Mac documentation](https://docs.espressif.com/projects/esp-idf/en/v3.1.5/get-started-cmake/macos-setup.html).
{: .notice--info}

### Install Prerequisites
ESP-IDF will use the version of Python installed by default on Mac OS.

install pip:
```bash
sudo easy_install pip
```
{: .code-offset-25}

install pyserial:
```bash
sudo pip install pyserial
```
{: .code-offset-25}

install CMake & Ninja build:

* If you have [HomeBrew](https://brew.sh/), you can run:

```bash
brew install cmake ninja
```
{: .code-offset-50}

* If you have [MacPorts](https://www.macports.org/install.php), you can run:

```bash
sudo port install cmake ninja
```
{: .code-offset-50}

* Otherwise, consult the [CMake](https://cmake.org/) and [Ninja](https://ninja-build.org/) home pages for Mac OS installation downloads.

It is strongly recommended to also install [ccache](https://ccache.samba.org/) for faster builds. If you have [HomeBrew](https://brew.sh/), this can be done via `brew install ccache` or `sudo port install ccache` on [MacPorts](https://www.macports.org/install.php).

{% capture notice-info %}
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun

Then you will need to install the XCode command line tools to continue. You can install these by running ``xcode-select --install``.
{% endcapture %}

<div class="notice--info">
  <strong>Note:</strong> If an error like this is shown during any step:
  {{ notice-info | markdownify }}
</div>


### Toolchain Setup
ESP32 toolchain for macOS is available for download from Espressif website:

[https://dl.espressif.com/dl/xtensa-esp32-elf-osx-1.22.0-80-g6c4433a-5.2.0.tar.gz](https://dl.espressif.com/dl/xtensa-esp32-elf-osx-1.22.0-80-g6c4433a-5.2.0.tar.gz)

Download this file, then extract it in ``~/esp`` directory:

```bash
mkdir -p ~/esp
cd ~/esp
tar -xzf ~/Downloads/xtensa-esp32-elf-osx-1.22.0-80-g6c4433a-5.2.0.tar.gz
```

The toolchain will be extracted into ``~/esp/xtensa-esp32-elf/`` directory.

To use it, you will need to update your ``PATH`` environment variable in ``~/.profile file``. To make ``xtensa-esp32-elf`` available for all terminal sessions, add the following line to your ``~/.profile`` file:

```bash
export PATH=$PATH:$HOME/esp/xtensa-esp32-elf/bin
```

Alternatively, you may create an alias for the above command. This way you can get the toolchain only when you need it. To do this, add different line to your ``~/.profile`` file:

```bash
alias get_esp32="export PATH=$PATH:$HOME/esp/xtensa-esp32-elf/bin"
```

Then when you need the toolchain you can type ``get_esp32`` on the command line and the toolchain will be added to your ``PATH``.

Log off and log in back to make the ``.profile`` changes effective. Run the following command to verify if ``PATH`` is correctly set:

```bash
printenv PATH
```

# Next Step

You can go ahead with [Lab 0]({{ '/labs/00-setup-the-code/' | relative_url }})
