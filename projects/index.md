---
layout: page
title: projects
nav_order: 3
has_children: true
permalink: /projects/index
has_toc: false
---

# Projects

## [mfa for macOS](./tools/xbar/mfa)
An [`xbar`](https://xbarapp.com/) plugin and stand-alone shell tool for interacting with TOTP mfa codes. The current version is built for YubiKey but the legacy version included can be used independently and only requires [oath-toolkit](https://www.nongnu.org/oath-toolkit/). 

## [mfa for Gnome](./tools/bitbar/mfa)
An [`argos`](https://github.com/p-e-w/argos) plugin and stand-alone shell tool for interacting with TOTP mfa codes, built for YubiKey. 

## [vscode extension pack](./extension-pack/info)
This extension pack was originally only intended for my own use but a few folks have found it handy. I'm frequently installing VSCode on various systems and was looking for a quick and easy way to ensure I always pulled the extensions I frequently use. Because this is an extension 'pack', you can easily dump any of the extensions you don't want or need. Having used this set of tools over a long period of time, I've never found it to bog down the app or cause any difficulty for me. 

## arcolinux customization [for xfce](./arco-xfce/info) and [gnome environments](./arco-gnome/info)
[Arcolinux](https://arcolinux.com/) is an arch linux distribution I like to experiment with. The maintainers tend to throw everything including the kitchen sink in as selectable options at install-time, and I've found it helpful to be able to compare their implementations for packages or solutions with my own. My repositories include post-setup shell scripts that assume a base set of options during OS install, then _fill in the gaps_ with the tools, packages, or customizations that I regularly work with. 