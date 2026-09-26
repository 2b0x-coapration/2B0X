# 2B0X — Virtual OS Launcher

**v1.0 (beta)**

A lightweight, VirtualBox-style desktop app for Linux that launches OS images —
ISO files, ZIP archives, or plain folders — as real virtual machines using
QEMU/KVM.

- **Upload** an ISO / ZIP / folder — 2B0X packages it into a bootable image
- **Launch** it with one click — boots in its own window via QEMU, with a
  branded loading animation while it starts up
- Each imported OS gets its own **persistent virtual disk**, so anything you
  do inside the VM (installs, saved files, settings) is kept between launches
- **History** tab tracks everything you've launched, with one-click re-launch
- Resizable window, dark UI, error dialogs instead of crashes

## Requirements

- Ubuntu/Debian-based Linux
- `python3-gi`, `gir1.2-gtk-3.0` (GTK3 UI)
- `qemu-system-x86`, `qemu-utils` (virtualization backend)
- `genisoimage` (packages folders/zips into bootable ISOs)
- `/dev/kvm` for hardware acceleration (optional but recommended)

All of these are declared as dependencies in `debian/control` and will be
installed automatically by `apt` when you install the `.deb`.

## Install

Grab a built `.deb` from [Releases](../../releases), or build it yourself:

```bash
git clone <this-repo-url>
cd 2b0x
./build.sh
sudo apt install ./2b0x_1.0~beta.deb
```

Launch **2B0X** from your applications menu, or run `2b0x` from a terminal.

## Uninstall

```bash
sudo apt remove 2b0x
```

Your imported OS images and virtual disks live in `~/.config/2b0x` and are
**not** removed automatically, so reinstalling later picks up where you left
off. To wipe them too:

```bash
rm -rf ~/.config/2b0x
```

## Repo layout

```
src/2b0x           the application (single Python/GTK3 script)
data/2b0x.desktop  desktop entry
data/icons/        app icon at multiple sizes
debian/control     package metadata / dependencies
debian/postinst    post-install hook (icon cache, desktop DB refresh)
build.sh           builds the .deb from the above
```

## Known limitations

- Booting an OS from a folder or ZIP works by packaging it into an ISO on
  the fly (`genisoimage`) — it can't magically make an arbitrary folder of
  files bootable if there's nothing bootable in it.
- The QEMU backend here targets x86/x86_64 guests. ARM-based OS images (e.g.
  many Android/LineageOS builds meant for phones/tablets) need
  `qemu-system-aarch64` with a different machine setup — not wired up in
  this version.
- No custom in-guest boot splash — that's controlled by the guest OS itself,
  not the hypervisor. 2B0X's own splash plays while QEMU is starting.

## License

MIT — see `LICENSE`.
