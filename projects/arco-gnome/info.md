---
layout: page
title: arco-gnome
parent: projects
nav_order: 4
permalink: /projects/arco-gnome/info
has_toc: false
---

# ArcoLinux Customization for Gnome

[jump to repo](https://github.com/bryandodd/arco-gnome){: .btn }

The scripts and config files presented here are intended solely for my personal use, but made available so that anyone stumbling across the repo should feel free to adapt them for their own use as well.

Scripts are intended to be executed in sequence with full reboots between executions.

You may need to make scripts executable ...
``` bash
$ sudo chmod +x 01-config-os.sh
```

---

### 01-config-os.sh _(run with sudo)_
* Removes several unneeded / unnecessary applications
* Installs tools used by this script and others to configure the system
* Installs icon and cursor themes
* Installs a couple of tools used for installing and managing gnome shell extensions
* Reverts the network adapter naming convention to standard
* Downloads custom config file for Kitty terminal
* Changes default shell from `bash` to `zsh`
* Installs fonts

### 02-software.sh _(run without sudo)_
* Configures preferred Gnome settings
* Sets several mimetypes to prefer Visual Studio Code _(recommended to install VSCode before running this script)_

### Install Gnome Extensions
Manually install preferred Gnome shell extensions. Those listed below are taken from [`https://extensions.gnome.org/`](https://extensions.gnome.org/). The easiest way to install these is by installing the [GNOME Shell Integration](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep) Google Chrome browser extension, then install the `chrome-gnome-shell` native host connector service:
``` bash
$ paru -Sy aur/chrome-gnome-shell-git --needed --noconfirm
```

* [Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)
* [ArcMenu](https://extensions.gnome.org/extension/3628/arcmenu/)
* [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
* [Transparent Top Bar](https://extensions.gnome.org/extension/1765/transparent-topbar/)
* [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
* [UTCClock](https://extensions.gnome.org/extension/1183/utcclock/)

Predefined settings for ArcMenu and Dash-to-Panel can be downloaded from this repo and imported into their respective extensions. Because of the settings used in these files, its recommended that you hold off applying these settings until you've run at least the `03-settings-and-software.sh` script.
* [ArcMenu Settings](https://raw.githubusercontent.com/bryandodd/arco-gnome/main/configs/arcmenu/arcmenu-settings.bak)
* [Dash-to-Panel Settings](https://raw.githubusercontent.com/bryandodd/arco-gnome/main/configs/dash-to-panel/dtp-settings.bak)

### 03-settings-and-software.sh
* Downloads repo config file for Neofetch
* Installs and configures Powerlevel10k for zsh
* Installs
    * Python 2 _(w/pip2 via manual install)_
    * Python 3 _(w/pip3)_
    * Flameshot
    * Visual Studio Code
    * Microsoft Teams
    * Slack Desktop
    * Remmina (RDP)
    * btop++
    * exa _(w/alias)_
    * bat _(w/alias)_
    * OAuth Toolkit
    * AWS CLI v2
    * kubectl
    * kubectx & kubens
    * AWS IAM Authenticator
    * eksctl
* Checks for and installs vendor-specific CPU microcode if not already installed

### 04-security-apps.sh
These applications are entirely optional and are primarily focused on security / penetration testing.

* Installs
    * Golang
    * Samba support
    * Impacket
    * nmap
    * OpenJDK v11
    * BurpSuite
    * Metasploit Framework
    * Custom Python scripts
    * Amass
    * WhatWeb
    * Nikto
    * DirBuster
    * GoBuster
    * SearchSploit
    * Nessus
    * PowerSploit
    * Hydra
    * Responder
    * MITM6