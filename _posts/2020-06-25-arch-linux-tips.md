---
layout: post
title: Arch Linux Tips
subtitle: Improving productivity in Arch Linux with i3
tags: [arch linux, tips]
comments: false
---


## Automount USB drives

Arch Linux does not create a `/media` directory by default. We need to create
that and then install the `udevil` package, which includes the `devmon` binary.
```
sudo mkdir /media
sudo pacman -S udevil
```

Next, we need to add the following line in the `.xinitrc` file before the call
to `exec i3`:
```
devmon &
```

References:
  * [][https://ignorantguru.github.io/udevil/]
  * [][https://igurublog.wordpress.com/downloads/script-devmon/]

