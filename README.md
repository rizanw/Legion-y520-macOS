# Legion-y520-macOS

> This repository is actually a buckup data for my macOS catalina on Lenovo Legion y520. But, anyone may use it for reference. Do it your own risks.

### ChangeLog
2020/04/04
- initial creation

##  Specification
### Machine :
- Intel Core i7-7700HQ Processor ( 2.81GHz 6MB )
- 8.0GB DDR4 SODIMM 2400MHz
- NVIDIA GeForce GTX1050Ti (Disabled)
- Intel HD 630 1536MB
- Qualcomm Atheros QCA6174A wifi
- Audio Realtek (ALC3248)
- Realtek Lan

### OS  :
- Mojave (10.14.6) : :tw-2705: Tested  
- Catalina (10.15.4) : :tw-2705: Tested

## What works
:tw-2705: UEFI booting via Clover.
:tw-2705:  Accelerated graphics for HD630 including OpenCL & Metal.
:tw-2705:  Native USB3/USB2/USB-C.
:tw-2705:  Native audio with AppleHDA, including headphones.
:tw-2705:  Built-in camera.
:tw-2705:  Native power management.
:tw-2705:  Battery status.
:tw-2705:  Built-in keyboard (with special function keys).
:tw-2705:  Built-in trackpad (basic gestures).
:tw-2705:  Backlight controls.
:tw-2705:  Wired Ethernet Lan.
:tw-2705:  Messages/FaceTime.
:tw-2705:  AirPlay mirroring to AppleTV.

## What does not work
:tw-274c: build-in WiFi 
:tw-274c: Bluetooth
:tw-274c: NVIDIA Graphic
:tw-274c: HDMI/DP video/audio don't work because those ports are connected to the NVIDIA, which is disabled.

### Solution
WiFi Networking & Bluetooth Hands-Off:
- Broadcom BCM94352Z (native wifi & Bluetooth)
- TP-Link TL-WN725N (wifi dongle) with [wireless adapter clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover.git "wireless adapter clover")
Graphics:
- Disable NVIDIA

## Necessary & Recomendation Kexts
- **[VirtualSMC + Plugins](https://github.com/acidanthera/VirtualSMC/releases "VirtualSMC + Plugins")** (the most required kext for hackintosh)
- **[Lilu](https://github.com/acidanthera/lilu/releases "Lilu")**
- **[WhateverGreen](https://github.com/acidanthera/WhateverGreen/releases "WhateverGreen")**
- **[AppleALC](https://github.com/acidanthera/applealc/releases "AppleALC")**
- **[VoodooPS2](https://github.com/acidanthera/VoodooPS2 "VoodooPS2")**
- **[VoodooTSCSync](https://github.com/RehabMan/VoodooTSCSync")**
- **[RealtekRTL8111](https://github.com/RehabMan/OS-X-Realtek-Network "RealtekRTL8111")** (for BCM94352Z)
- **[AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup/releases "AirportBrcmFixup")** (for BCM94352Z)
- **[RtWlanU1827 & RtWlanU](https://github.com/chris1111/Wireless-USB-Adapter-Clover "RtWlanU1827 & RtWlanU")** (for TL-WN725N)

## Preparing & Initial Installation
follow this instruction very well [[Guide] Booting the OS X installer on LAPTOPS with Clover](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/ "[Guide] Booting the OS X installer on LAPTOPS with Clover")
with **Notes :**
- Use newest **[clover](https://github.com/CloverHackyColor/CloverBootloader/releases "clover")**
- Download official macOS ([Catalina](https://support.apple.com/en-us/HT201475 "Catalina") / [Mojave](https://support.apple.com/en-us/HT210190 "Mojave")) from App Store
- Use the 'createinstallmedia' approach
- Use the option 1 "MBR with a FAT32 partition for Clover and a separate HFS+J partition for the OS X installer." much better.
- Use the config.plist from this repo
- Replace the drivers with this repo drivers (used clover version r5108)
- Put the hotpatch to ACPI/patched/

#### BIOS configuration :
- Set BIOS to defaults.
- UEFI boot is enabled.
- Secure boot is disabled.
- Fast boot is disabled.
- SATA mode is set to AHCI.
- Intel Virtualization Technology is enabled.

## Installation
do as installing macOS in general.

## Post Installation
- use `mount_efi.sh` in tools to mount the efi
- replace EFI folder in EFI Volumes from your USB EFI
- Install the [wireless adapter clover](https://github.com/chris1111/Wireless-USB-Adapter-Clover.git "wireless adapter clover") (for TL-WN725N)

#### References : 
- [[Guide] Booting the OS X installer on LAPTOPS with Clover](https://www.tonymacx86.com/threads/guide-booting-the-os-x-installer-on-laptops-with-clover.148093/ "[Guide] Booting the OS X installer on LAPTOPS with Clover")
- [Hackintosh Vanilla Desktop Guide](https://hackintosh.gitbook.io/-r-hackintosh-vanilla-desktop-guide/ "Hackintosh Vanilla Desktop Guide")
- [[Guide] Lenovo Legion Y520/Y720 using Clover UEFI](https://www.tonymacx86.com/threads/guide-lenovo-legion-y520-y720-using-clover-uefi.261009/ "[Guide] Lenovo Legion Y520/Y720 using Clover UEFI")
- [Some Clover config.plist files for common Intel graphics configurations](https://github.com/RehabMan/OS-X-Clover-Laptop-Config "Some Clover config.plist files for common Intel graphics configurations")
- [[Guide] How to Boot macOS Installer on Laptops using Clover (UEFI/Legacy)](https://www.elitemacx86.com/threads/guide-how-to-boot-macos-installer-on-laptops-using-clover-uefi-legacy.177/ "[Guide] How to Boot macOS Installer on Laptops using Clover (UEFI/Legacy)")



