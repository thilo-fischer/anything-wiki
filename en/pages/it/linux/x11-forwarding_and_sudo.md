# SSH X11 Forwarding

https://unix.stackexchange.com/a/12772

`echo ForwardX11 yes > ~/.ssh/config`

# X11 Forwarding With sudo/su

## Problem Description And Solution Using Xauth

https://blog.mobatek.net/post/how-to-keep-X11-display-after-su-or-sudo/

## Quick Workaround

- Start SSH session as normal user with X11 forwarding.
- Start `xterm` or another X11 terminal from that session.
- Run `sudo` or `su` from the new terminal window.
