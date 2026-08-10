<img src="assets/argus.svg" width="72" align="right" alt="">

# Argus

**A screen-edge service-status sidebar for KDE on Wayland.**

A thin rail sits against your screen edge showing a status dot per service.
Hover and it expands into a list with favicons and names; click one and it opens
in your browser. Green is up, red is down, and you can see it without
alt-tabbing to a dashboard.

It pulls from [Uptime&nbsp;Kuma](https://github.com/louislam/uptime-kuma), from
its own HTTP checks, or from both at once. It lives in the system tray and has a
Settings page, so adding a service never means editing a file by hand.

## Try it

**[▸ Interactive demo at jatify.com](https://jatify.com)** — select *Argus* from
the project row, then hover the rail at the right edge. You can reveal the
service list, see the live up/down dots, and click through to a service.

It's a faithful browser recreation of the interface, not the app itself — the
real thing is a native Wayland client.

## Features

- **Two sources, one rail** — Uptime Kuma monitors and/or manual links that
  Argus health-checks itself. Use either, or mix them.
- **Self-contained checks** — manual links are pinged on their own interval
  (default 60s); 2xx/3xx counts as up. No Kuma instance required.
- **Favicons** — each service shows its own icon, fetched automatically.
- **Two collapsed styles** — `dots` (status dots peek out at the edge, so you
  can see an outage without hovering) or `edge` (a thin accent strip).
- **Either edge** — left or right, your choice.
- **Credentials stay out of the file** — set `password_command` and Argus reads
  your Kuma password from `secret-tool`, `pass`, or anything else on stdout.

## Install

Download the package for your distribution from the
[**Releases**](../../releases) page, then:

```bash
# Arch
sudo pacman -U argus-*.pkg.tar.zst

# Debian / Ubuntu
sudo dpkg -i argus_*.deb

# Fedora
sudo dnf install ./argus-*.rpm
```

Then create your config:

```bash
mkdir -p ~/.config/argus
cp /usr/share/doc/argus/config.example.toml ~/.config/argus/config.toml
chmod 600 ~/.config/argus/config.toml
```

You need **at least one source** — the Uptime Kuma keys, one or more
`[[services]]` blocks, or both. Then run `argus`. It autostarts on login (toggle
in **KDE → Autostart**) and appears in the system tray: Show/Hide · Open
Uptime-Kuma · Settings… · Quit.

## Requirements

KDE Plasma 6 on Wayland (kwin). All dependencies are official distribution
packages and are pulled in by the package: `pyside6`, `python-socketio`,
`python-requests`, `python-tomlkit`, `qt6-wayland`, `qt6-svg`, `layer-shell-qt`,
`kirigami`.

## Configuration

`~/.config/argus/config.toml`. A commented reference ships as
[`config.example.toml`](config.example.toml).

### Uptime Kuma (optional)

| Key | Description |
|---|---|
| `kuma_url` | Base URL of your Uptime Kuma instance |
| `username` | Kuma username |
| `password` | Kuma password — **or** use `password_command` instead |
| `password_command` | Shell command printing the password on stdout, e.g. `secret-tool lookup argus kuma_password` |

### `[[services]]` — manual links (optional)

| Key | Required | Default | Description |
|---|---|---|---|
| `name` | yes | — | Display label |
| `url` | yes | — | URL to open, and to health-check |
| `interval` | no | `60` | Seconds between checks |

### Appearance

| Key | Default | Description |
|---|---|---|
| `edge` | `"right"` | Screen edge: `"left"` or `"right"` |
| `collapsed_style` | `"dots"` | Collapsed look: `"dots"` (peeking status dots) or `"edge"` (thin accent strip) |
| `collapsed_px` | `24` | Width of the collapsed strip |
| `expanded_px` | `260` | Rail width when revealed |
| `icon_px` | `24` | Favicon size inside a tile |
| `reveal_delay_ms` | `0` | Delay before revealing on hover |
| `hide_delay_ms` | `250` | Delay before hiding after the pointer leaves |

## Usage

- **Reveal** — hover over the rail at the screen edge.
- **Open a service** — click its tile.
- **Tray menu** — Show/Hide, Open Uptime-Kuma, Settings…, Quit.
- **Settings page** — add, edit and remove services and adjust appearance;
  changes are written back to `config.toml` with your comments preserved.

## Source

Argus is closed source for now. This repository carries the documentation and
the release downloads.

Bug reports and feature requests are welcome in
[Issues](../../issues) — please include your distribution, Plasma version, and
whether you're on Wayland or X11.

## License

Copyright © 2026 Alexandru Martalogu. All rights reserved.

The published packages are distributed for personal use. They are not open
source, and no license to the source code is granted.
