---
title: "Wine Proton Custom"
date: 2022-02-17
---

[Disco Elysium](https://en.wikipedia.org/wiki/Disco_Elysium) has stopped working. The game freezes on loading. A solution is to install a custom build of proton, [GloriousEggroll/proton-ge-custom](https://github.com/GloriousEggroll/proton-ge-custom). It has FFmpeg enabled for FAudio, patches from wine-staging and Vulkan for Direct3D (VKD3D). 

Download a [release](https://github.com/GloriousEggroll/proton-ge-custom/releases)

``` shell
mkdir -v ~/.steam/root/compatibilitytools.d/
tar -xf Proton-7.2-GE-2.tar.gz -C ~/.steam/root/compatibilitytools.d/
```
<!--more-->
