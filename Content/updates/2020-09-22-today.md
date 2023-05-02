---
date: "2020-09-22"
title: "yay AUR helper"
---

[Yay]() is a command-line tool to install and manage software from the [Arch User Repository](). It is included in the [_community_](https://discover.manjaro.org/packages/yay) repository of manjaro Linux.

``` shell
> pacman -Ss yay
community/yay 10.0.4-1
    Yet another yogurt. Pacman wrapper and AUR helper written in go.

> sudo pacman -S yay
> man yay
> yay --help

# upgrade system
> yay -Syyu 

# remove stale packages
> yay -Qdt
> yay -R $(yay -Qdtq | xargs)

# cleanup
> yay -Sc --noconfirm

# edit PKGBUILD
> mkdir ~/AUR_local && cd ~/AUR_Local
> yay --getpkgbuild julius-game
:: Querying AUR...
:: Downloaded PKGBUILD (1/1): julius-game
> ls julius-game/
total 12K
-rw-r--r-- 1 user0 user0  173 Sep 22 11:04 julius-game.desktop
-rw-r--r-- 1 user0 user0  145 Sep 22 11:04 julius-game.install
-rw-r--r-- 1 user0 user0 1.4K Sep 22 11:04 PKGBUILD
> cd julius-game
> vi PKGBUILD
> makepkg -si

# show foreign packages
# i.e those not belonging to official repos
# yay -Q --foreign
> yay -Qm

# info about AUR package
> yay -Si ktlint
:: Querying AUR...
Repository      : aur
Name            : ktlint
Keywords        : None
Version         : 0.38.1-1
Description     : An anti-bikeshedding Kotlin linter with built-in formatter
URL             : https://ktlint.github.io/
AUR URL         : https://aur.archlinux.org/packages/ktlint
....
```

<!--more-->
