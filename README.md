# Amber's Dynamic-Color DWL Package

This repository is a personal Arch package source for `dwl` 0.7. It keeps the
simple suckless-style local build workflow, but adds the pieces needed to make a
Wayland session feel closer to a tuned `dwm` setup: a top bar, familiar
keybindings, a dmenu replacement, clickable tags, and live wallpaper-derived
colors.

It intentionally uses the released `dwl` 0.7 tarball, not `dwl-git`.

## What This Adds Over Plain DWL

- A custom `config.h` with Alt-based dwm-style bindings, XWayland enabled,
  sloppy focus, `Alt+p` launcher, `Alt+Shift+w` Zen Browser, `Alt+b` bar
  toggle, `Alt+n` tag naming, and `Alt+F5` wallust refresh.
- `dwl-runtime-colors.patch`, which teaches `dwl` to read
  `~/.cache/wallust/dwm.Xresources` and reload root/border colors on `SIGUSR1`.
- `dwl-ipc.patch` plus `dwl-ipc-unstable-v2.xml`, which add the IPC protocol
  needed by `dwlb -ipc` so tag clicks on the bar switch tags.
- A `dwlb` top bar launcher and status updater that read the same wallust color
  file as the compositor.
- Persistent `Alt+n` tag names stored in `~/.config/dwl/tag-names`, shown by
  `dwlb` while preserving clickable IPC tags.
- A `bemenu` launcher wrapper that combines desktop applications, executable
  commands, zsh aliases/functions, recent zsh history, and typed shell snippets.
- A Nitrogen compatibility wrapper for picking wallpaper while `swaybg` displays
  it under Wayland.
- A wallust refresh command that updates the wallpaper, compositor colors,
  `dwlb`, GTK/Qt/Kvantum dark theme files, and Zen Browser chrome CSS from the
  selected wallpaper.
- A generated `Wallust-Dark` GTK theme under `~/.themes` so LXAppearance shows
  the active wallust theme as a selectable named theme instead of Adwaita.
- Dark-by-default GTK and Qt environment defaults for new applications launched
  inside the session.

## Build And Install

Build and install the local package:

```sh
makepkg -si
```

Or build first, then install with pacman:

```sh
makepkg -f
sudo pacman -U ./dwl-0.7-*.pkg.tar.zst
```

The package installs:

- `dwl` to `/usr/bin/dwl`
- public commands to `/usr/bin`: `start-dwl`, `dwl-wallust-refresh`,
  `dwl-bemenu-run`
- session-private helpers to `/usr/lib/dwl-personal`

Launch the session with:

```sh
start-dwl
```

## Runtime Expectations

The color workflow expects wallust to write:

```text
~/.cache/wallust/dwm.Xresources
```

The wallpaper workflow follows Nitrogen's saved wallpaper file:

```text
~/.config/nitrogen/bg-saved.cfg
```

Press `Alt+F5` inside DWL after changing wallpaper, or run:

```sh
dwl-wallust-refresh
```

That command reruns wallust, updates toolkit themes, regenerates the
LXAppearance-visible `Wallust-Dark` GTK theme, restarts `swaybg`, signals `dwl`
with `SIGUSR1`, and restarts `dwlb`.

Press `Alt+n` to name the currently selected tag. Submitting an empty name
resets that tag to its number. The names persist across bar refreshes and DWL
restarts.

Zen Browser profiles are auto-detected under `~/.zen`. If Zen has created a
profile, the refresh command writes `chrome/dwl-wallust.css`, imports it from
`chrome/userChrome.css`, and enables browser chrome CSS in `user.js`. Restart
Zen after refreshing colors so the browser chrome picks up the new selected bar
accent.

Recommended optional runtime packages:

```sh
sudo pacman -S --needed dwlb bemenu wallust swaybg nitrogen qt5ct qt6ct kvantum kvantum-qt5 nwg-look adw-gtk-theme
```

## Notes

Generated tarballs, build directories, and local package artifacts are ignored.
Commit the package source files, not built `.pkg.tar.zst` files.
