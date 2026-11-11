# Legion Y520 macOS (Clover EFI)

> [!NOTE]
> **Archived.** This is the EFI setup I used to run macOS Mojave and Catalina on my Lenovo Legion Y520 in 2020. It's no longer maintained and is shared for reference only. Use it at your own risk.
> Clover has since been replaced by [OpenCore](https://dortania.github.io/OpenCore-Install-Guide/) as the standard hackintosh bootloader. If you're starting fresh, follow the Dortania guide instead.

Clover configuration, ACPI hotpatch, and UEFI drivers to boot macOS on the Lenovo Legion Y520 with Intel HD 630 graphics, power management, audio, and USB working.

## Hardware

| Component | Model | Status |
|---|---|---|
| CPU | Intel Core i7-7700HQ (Kaby Lake) | ✅ native power management |
| iGPU | Intel HD Graphics 630 | ✅ accelerated, Metal and OpenCL |
| dGPU | NVIDIA GeForce GTX 1050 Ti | ❌ disabled (no macOS driver) |
| RAM | 8 GB DDR4-2400 | ✅ |
| Audio | Realtek ALC3248 | ✅ via AppleALC, including headphones |
| Ethernet | Realtek RTL8111 | ✅ |
| Wi-Fi / Bluetooth | Qualcomm Atheros QCA6174A | ❌ not supported, see [Wi-Fi](#wi-fi-and-bluetooth) |
| HDMI / DisplayPort | wired to the NVIDIA GPU | ❌ |

SMBIOS: `MacBookPro14,3`

## Tested on

- macOS Mojave 10.14.6 ✅
- macOS Catalina 10.15.4 ✅
- Clover r5108

## What works

- UEFI boot via Clover
- Accelerated graphics on HD 630, including Metal and OpenCL
- USB 3, USB 2, and USB-C
- Audio with AppleHDA, including headphones
- Built-in camera
- Native CPU power management and battery status
- Keyboard with function keys, trackpad with basic gestures
- Backlight controls
- Wired Ethernet
- iMessage, FaceTime, and AirPlay mirroring (requires your own serial numbers, see [Post-installation](#post-installation))

## What doesn't work

- **Built-in Wi-Fi and Bluetooth**: the Qualcomm card has no macOS driver.
- **NVIDIA GPU**: disabled; Pascal GPUs have no driver after High Sierra.
- **HDMI and DisplayPort**: both are wired to the NVIDIA GPU.

### Wi-Fi and Bluetooth

- Replace the card with a **Broadcom BCM94352Z** for native Wi-Fi and Bluetooth (with AirportBrcmFixup), or
- use a **TP-Link TL-WN725N** USB dongle with [Wireless-USB-Adapter-Clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover).

## Repository contents

```
config/config.plist     Clover configuration (serial numbers removed, generate your own)
hotpatch/SSDT-Y520.aml  ACPI hotpatch, goes in EFI/CLOVER/ACPI/patched/
drivers/UEFI/           Clover UEFI drivers (ApfsDriverLoader, AptioMemoryFix, HFSPlus, FSInject)
tools/mount_efi.sh      mounts the EFI partition (based on CloverPackage's MountESP)
```

## Kexts

Download the latest release of each and place them in `EFI/CLOVER/kexts/Other/`:

| Kext | Purpose |
|---|---|
| [Lilu](https://github.com/acidanthera/Lilu/releases) | patching framework required by most kexts below |
| [VirtualSMC](https://github.com/acidanthera/VirtualSMC/releases) + plugins | SMC emulation, battery, sensors |
| [WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases) | Intel graphics |
| [AppleALC](https://github.com/acidanthera/AppleALC/releases) | audio |
| [VoodooPS2Controller](https://github.com/acidanthera/VoodooPS2/releases) | keyboard and trackpad |
| [VoodooTSCSync](https://bitbucket.org/RehabMan/VoodooTSCSync/downloads/) | TSC synchronization |
| [RealtekRTL8111](https://bitbucket.org/RehabMan/os-x-realtek-network/downloads/) | Ethernet |
| [AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases) | only with a BCM94352Z card |
| [RtWlanU / RtWlanU1827](https://github.com/chris1111/Wireless-USB-Adapter-Clover/releases) | only with a TL-WN725N dongle |

## BIOS settings

- Load defaults
- UEFI boot: enabled
- Secure Boot: disabled
- Fast Boot: disabled
- SATA mode: AHCI
- Intel Virtualization Technology: enabled

## Installation

1. Download macOS [Catalina](https://support.apple.com/en-us/HT201475) or [Mojave](https://support.apple.com/en-us/HT210190) from the App Store and create a USB installer with `createinstallmedia`.
2. Install [Clover](https://github.com/CloverHackyColor/CloverBootloader/releases) to the USB drive, following [Booting the OS X installer on laptops with Clover](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/). The MBR layout (FAT32 partition for Clover plus an HFS+J partition for the installer) worked best.
3. Copy `config.plist` from this repo, replace the drivers with the ones in `drivers/UEFI/`, add the kexts, and put `SSDT-Y520.aml` in `ACPI/patched/`.
4. Boot from USB and install macOS as usual.

## Post-installation

1. Mount the EFI partition of your system disk with `tools/mount_efi.sh` and copy the EFI folder from the USB drive into it.
2. **Generate your own SMBIOS** serial number, board serial (MLB), and SmUUID for `MacBookPro14,3` with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS), and replace the `GENERATE_YOUR_OWN_*` placeholders in `config.plist`. Never reuse serials from someone else's config: it breaks iMessage and FaceTime and can get the serial blocked by Apple.
3. If you use the TL-WN725N dongle, install [Wireless-USB-Adapter-Clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover).

## References

- [[Guide] Lenovo Legion Y520/Y720 using Clover UEFI](https://www.tonymacx86.com/threads/guide-lenovo-legion-y520-y720-using-clover-uefi.261009/)
- [RehabMan's OS-X-Clover-Laptop-Config](https://github.com/RehabMan/OS-X-Clover-Laptop-Config)
- [Hackintosh Vanilla Desktop Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/)
- [[Guide] How to boot the macOS installer on laptops using Clover](https://www.elitemacx86.com/threads/guide-how-to-boot-macos-installer-on-laptops-using-clover-uefi-legacy.177/)

## Credits

Thanks to [Adrian Jagielak](https://github.com/adrianjagielak) for fixing the kext links, and to the acidanthera, RehabMan, and Clover teams whose work this setup builds on.

## License

The configuration, hotpatch, and documentation are [MIT](LICENSE) licensed. The UEFI drivers and `mount_efi.sh` come from the Clover project and keep their original licenses.
