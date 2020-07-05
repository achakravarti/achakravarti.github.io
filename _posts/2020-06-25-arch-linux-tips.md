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

## Install a package with all optional dependencies

When we use `pacman` to install a package, the optional dependencies of the
package are not installed by default. In order to do so, we need to:
  1. install the package, and
  2. install the optional dependencies.

The `wine` package has a large number of optional dependencies, and is used as
an example below:
```
sudo pacman -S wine
sudo pacman -S --asdeps $(pactree -l wine)
```

Installing the optional dependencies iwth the `--asdeps` flag has the advantage
that the optional packages will be orphaned when the `wine` package is removed,
and can be easily removed with:
```
sudo pacman -Rns $(pacman -Qtdq)
```

References:
  * <https://ignorantguru.github.io/udevil/>
  * <https://igurublog.wordpress.com/downloads/script-devmon/>
  * <https://confluence.jaytaala.com/display/TKB/Install+a+package+with+all+optional+dependencies+in+Arch+based+distros>

