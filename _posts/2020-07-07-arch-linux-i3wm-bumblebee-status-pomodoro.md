---
layout: post
title: Setting up Pomodoro Module in Bumblebee Status
subtitle: Part of the series on Arch Linux
tags: [arch-linux, i3wm, bumblebee-status, pomodoro]
comments: false
---


## Tl;dr

```
sudo pacman -S dunst
mkdir -p ~/.config/dunst
cp /usr/share/dunst/dunstrc ~/.config/dunst/dunstrc
touch ~/.config/i3/pomodoro-notify.sh
chmod a+x ~/.config/i3/pomodoro-notify.sh
```

In `~/.config/i3/pomodoro-notify.sh`:
```
SOUND="/usr/share/sounds/freedesktop/stereo/alarm-clock-elapsed.oga"
notify-send Pomodoro "Take a break!" & paplay $SOUND
```

In `~/.config/i3/config`:
```
exec dunst
```

## References

  * <https://bumblebee-status.readthedocs.io/en/latest/modules.html#pomodoro>
  * <https://github.com/tobi-wan-kenobi/bumblebee-status/issues/532>
  * <https://www.reddit.com/r/i3wm/comments/ag0xac/notifysend_not_appearing/>
  * <https://www.reddit.com/r/i3wm/comments/b7q1sk/dunst/>
  * <https://wiki.archlinux.org/index.php/Dunst>

