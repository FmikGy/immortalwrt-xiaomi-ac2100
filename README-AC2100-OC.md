# Xiaomi Mi Router AC2100 1100 MHz firmware

This branch adds an opt-in MT7621 CPU PLL setting for the Xiaomi Mi Router
AC2100. The CPU target is 1100 MHz and the bus clock rises to 275 MHz.

## Build from GitHub Actions

1. Open the repository's **Actions** page.
2. Select **Build Xiaomi AC2100 1100MHz Firmware**.
3. Select **Run workflow**.
4. Leave **Create a GitHub Release** enabled to publish the images, or disable
   it to create only a temporary Actions artifact.
5. Keep **prerelease** enabled until the firmware has passed cold-boot and
   sustained-load tests on the target router.

Each run builds and verifies two variants in parallel:

- `wifi` includes the MT7603 and MT7615 drivers, firmware and wireless tools.
- `nowifi` keeps LuCI but removes the wireless drivers, firmware, `wpad`, `iw`,
  Wi-Fi scripts and the wireless regulatory database.

Both variants also share `configs/ac2100-common.config` and include:

- OpenSSH server replacing Dropbear
- root login and password authentication enabled for OpenSSH
- OpenSSH SFTP server
- WireGuard kernel/tools plus `luci-proto-wireguard`
- `luci-app-ttyd`
- `luci-app-upnp`
- [`luci-theme-aurora`](https://github.com/eamonxg/luci-theme-aurora)
- runtime opkg feeds switched to the [PKU ImmortalWrt mirror](https://mirrors.pku.edu.cn/immortalwrt/)

OpenSSH password login requires a non-empty root password on the router.
The opkg mirror switch runs on first boot and keeps a `.bak` copy of
`/etc/opkg/distfeeds.conf`.

When no release tag is supplied, the workflow uses
`ac2100-24.10-oc-r<run-number>`. Every successful build is retained as an
Actions artifact for 14 days. Published releases contain both variants, their
manifests and build information, plus per-variant and combined SHA-256 files.
The firmware filenames contain either `wifi` or `nowifi` before the image type.

Use the `sysupgrade.bin` image only when upgrading an already compatible
OpenWrt or ImmortalWrt installation. The separate `kernel1.bin` and
`rootfs0.bin` images are intended for installation or recovery procedures that
explicitly require those partition images.

## Warning

This is unofficial overclocked firmware. The workflow verifies the source
patch, generated device tree and image checksums, but it cannot verify hardware
stability. Keep serial or bootloader recovery available and test repeated cold
boots and sustained network load before production use.
