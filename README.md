# OpenCore running on MSI CX61-2PC
**Note:** This is not a guide, merely and aid as to what this model requires. For a full guide, see here: [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)

<p align="center">
  <img src="/images/about-this-mac.jpg">
</p>

Hardware Specs:

```
MODEL:      MSI CX61 2PC-1214XES
CPU:        Intel® Core™ i7-4712MQ
RAM:        16GB 1600 MHz DDR3
GPU:        Intel® HD Graphics 4600
            NVIDIA© Geforce 820M (Disabled)
AUDIO:      Realtek AC269
WIFI:       Intel® Dual Band Wireless-AC 7260
ETHERNET:   Qualcomm Atheros© E2200
```

## What's working

* macOS Big Sur, Monterey and Ventura
* HDMI Output
* Mic, speakers and headphones
* CPU Power Management
* Trackpad gestures
* Bluetooth and Wifi
  * Card was swapped from RTL8723AE
* Battery readouts
* Front webcam
* Micro SD card reader
* Sleep
* CPU fan control
* Brightness Keys
  * A [custom map](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Use-Custom-Keyboard-Mapping) must be applied 

## What's not working

* NVIDIA© Geforce 820M
  * Disabled for not being compatible
* Special buttons (Eject, Screen Off, Turbo Mode, SCM, Airplane Mode)
  * A [custom map](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Use-Custom-Keyboard-Mapping) can be applied to make them working, but some keys like eject button cannot be mapped as they work in a different way than Apple expects

## BIOS Settings

For the most part it's pretty stock, main guys you need to change:

* Disable CFG Lock (https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock)
* Change video memory size to 64MB
  * When the default video memory size is set, the internal screen it's totally unusuable

## ACPI

For the those curious, I've also provided an ACPI dump of my laptop

* [Firmware Dumps](/ACPI/ACPI-Dumps/)
* [Custom SSDTs](/ACPI/Custom-SSDTs/)

**Required SSDTs**
| SSDT | ACPI Patches | Comments |
| :--- | :--- | :--- |
| SSDT-FANQ | N/A | Compiled version of MSI Fan Service's kext |
| SSDT-FN | N/A | Maps brightness keys to F14 & F15 |
| SSDT-GPRW | `GPRW` to `XPRW`| Fixes wake up from sleeping |
| SSDT-HPET | Some replaces must be done | Fixes IRQ Conflicts |
| SSDT-NoHybGfx | N/A | Disables discrete graphics card |
| SSDT-PLUG | N/A | Fixes power management |
| SSDT-PNLF | N/A | Add Backlight control support 

For a full list of ACPI patches, see here: [patches.plist](/ACPI/Custom-SSDTs/patches.plist)

## Kexts

Hardware specific kexts:

* [Airportltlwm](https://github.com/OpenIntelWireless/itlwm)
* [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM)
  * Bluetooth cannot be enabled without it
* [MSIFanService](https://github.com/lgs3137/MSIFanControl/)
  * MSIECControl or MSIFanControl apps can be used to control CPU fan's speed.
    A kext and apps compiled versions can be found in [https://github.com/ivansoriarab/Compiled-MSI-Fan-Control/](Compiled MSI Fan Control)
* [VoodooPS2](https://github.com/acidanthera/VoodooPS2)
