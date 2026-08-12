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

**[▸ Interactive demo](https://jatify.com/#argus)** — hover the rail at the right
edge. You can reveal the service list, see the live up/down dots, and click
through to a service.

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
sudo pacman -U argus-sidebar-*.pkg.tar.zst

# Debian / Ubuntu
sudo dpkg -i argus-sidebar_*.deb

# Fedora
sudo dnf install ./argus-sidebar-*.rpm
```

> The package is called **argus-sidebar**, and the command it installs is
> `argus-sidebar`. The name `argus` already belongs to an unrelated network
> monitor that ships its own `/usr/bin/argus`, so this package stays out of its
> way — the two can be installed side by side.

### Verify what you downloaded

Every release is signed. `SHA256SUMS` covers the packages, and `SHA256SUMS.asc`
is a clearsigned copy of it, so checking the signature and then the checksums
is enough to trust all four files.

```bash
gpg --import argus.pub             # published with every release
gpg --verify SHA256SUMS.asc        # expect: Good signature from "Argus CI"
sha256sum -c SHA256SUMS
```

Each artifact also ships its own detached `.asc` if you'd rather check one
directly: `gpg --verify argus-sidebar_0.1.0_all.deb.asc argus-sidebar_0.1.0_all.deb`.

The signing key fingerprint is `C86B FDAF FA81 D34D 81F9  4926 5F18 6C35 C3CF 7A75`.

Then create your config:

```bash
mkdir -p ~/.config/argus
cp /usr/share/doc/argus-sidebar/config.example.toml ~/.config/argus/config.toml
chmod 600 ~/.config/argus/config.toml
```

You need **at least one source** — the Uptime Kuma keys, one or more
`[[services]]` blocks, or both. Then run `argus-sidebar`. It autostarts on login (toggle
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

Argus is **source-available, not open source.**

The release packages contain the Python and QML source, and so does the release
tarball. That is unavoidable for a PySide6 application — the interpreter needs
the source to run it — and the tarball is what the AUR package builds from. Being
able to read it grants you no rights over it: you may run it under the terms in
[LICENSE](LICENSE), and nothing else. Redistribution, modification and derivative
works are not permitted. See sections 3 and 4.2 in particular.

If you want to use any of this in your own project, ask — the address is at the
bottom of the licence.

This repository carries the documentation and the release downloads.

Bug reports and feature requests are welcome in
[Issues](../../issues) — please include your distribution, Plasma version, and
whether you're on Wayland or X11.

## License

Copyright © 2026 Alexandru Martalogu. All rights reserved.

Argus is proprietary software, licensed for personal use under the terms in
[LICENSE](LICENSE). It is **not** open source, and no licence to the source code
is granted beyond the rights that document sets out.

The packages contain Python source as a consequence of the language; that is not
a publication of the source, and does not place it under any open source licence.
See section 4.2 of the licence.
