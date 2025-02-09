---
title: Arcolinux Setup for Gnome
icon: simple/gnome
---
# Arcolinux Gnome Setup

!!! blankout inline end ""

    [:simple-github: Repo on GitHub :material-arrow-top-right:](https://github.com/bryandodd/arco-gnome){ .button-62 .md-button--primary }

The scripts and config files presented here are intended solely for my personal use, but made available so that anyone curious enough should feel free to adapt them for their own use as well.

* Scripts are intended to be executed in sequence with full reboots between executions.
* You may need to make scripts executable: `#!bash sudo chmod +x 01-config-os.sh`

!!! warning "Scripts Outdated"

    The last update to this repo was May 7, 2022. Many things have likely changed since then.

## Script 1: Initial OS Config | [`01-config-os.sh`](https://github.com/bryandodd/arco-gnome/blob/main/01-config-os.sh)

* Removes several unneeded / unnecessary applications
* Installs tools used by this script and others to configure the system
* Installs icon and cursor themes
* Installs a couple of tools used for installing and managing gnome shell extensions
* Reverts the network adapter naming convention to standard
* Downloads custom config file for Kitty terminal
* Changes default shell from `bash` to `zsh`
* Installs fonts

## Script 2: Gnome Preferences | [`02-gnome-prefs.sh`](https://github.com/bryandodd/arco-gnome/blob/main/02-gnome-prefs.sh)

* Configures preferred Gnome settings
* Sets several mimetypes to prefer Visual Studio Code _(recommended to install VSCode before running this script)_

## Install Gnome Extensions

Manually install preferred Gnome shell extensions. Those listed below are taken from [`https://extensions.gnome.org/`](https://extensions.gnome.org/). The easiest way to install these is by installing the [GNOME Shell Integration](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep) Google Chrome browser extension, then install the `chrome-gnome-shell` native host connector service:

``` shell-session
$ paru -Sy aur/chrome-gnome-shell-git --needed --noconfirm
```

* [Dash to Panel](https://extensions.gnome.org/extension/1160/dash-to-panel/)
* [ArcMenu](https://extensions.gnome.org/extension/3628/arcmenu/)
* [Vitals](https://extensions.gnome.org/extension/1460/vitals/)
* [Transparent Top Bar](https://extensions.gnome.org/extension/1765/transparent-topbar/)
* [Clipboard Indicator](https://extensions.gnome.org/extension/779/clipboard-indicator/)
* [UTCClock](https://extensions.gnome.org/extension/1183/utcclock/)

Predefined settings for ArcMenu and Dash-to-Panel can be downloaded from this repo and imported into their respective extensions. Because of the settings used in these files, its recommended that you hold off applying these settings until you've run at least the `03-settings-and-software.sh` script.

## Script 3: Settings and Software | [`03-settings-and-software.sh`](https://github.com/bryandodd/arco-gnome/blob/main/03-settings-and-software.sh)

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

## Script 4: Security Apps | [`04-security-apps.sh`](https://github.com/bryandodd/arco-gnome/blob/main/04-security-apps.sh)

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
