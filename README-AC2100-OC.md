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

When no release tag is supplied, the workflow uses
`ac2100-24.10-oc-r<run-number>`. Every successful build is retained as an
Actions artifact for 14 days. Published releases contain the firmware images,
manifest, build information, profiles and SHA-256 checksums.

Use the `sysupgrade.bin` image only when upgrading an already compatible
OpenWrt or ImmortalWrt installation. The separate `kernel1.bin` and
`rootfs0.bin` images are intended for installation or recovery procedures that
explicitly require those partition images.

## Warning

This is unofficial overclocked firmware. The workflow verifies the source
patch, generated device tree and image checksums, but it cannot verify hardware
stability. Keep serial or bootloader recovery available and test repeated cold
boots and sustained network load before production use.
