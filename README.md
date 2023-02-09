# OpenCore running on MSI CX61-2PC
**Note:** This is not a guide, merely and aid as to what this model requires. For a full guide, see here: [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)

<p align="center">
  <img src="/images/about-this-mac.jpg">
</p>

Hardware Specs:

```
MODEL       MSI CX61 2PC-1214XES
CPU:        Intel® Core™ i7-4712MQ
RAM:        16GB 1600 MHz DDR3
GPU:        Intel® HD Graphics 4600
            NVIDIA© Geforce 820M (Disabled)
AUDIO:      Realtek AC269
WIFI:       Intel® Dual Band Wireless-AC 7260
ETHERNET:   Qualcomm Atheros© E2200
```

## What's working

* HDMI Output
* Mic, speakers and headphones
* CPU Power Management
* Trackpad gestures
* Bluetooth and Wifi
* Battery readouts
* Front webcam
* Micro SD card reader
* Sleep
* CPU fan control
* Brightness Keys

## What's not working

* **NVIDIA© Geforce 820M**
  > Disabled for not being compatible
* **Special buttons (Eject, Screen Off, Turbo Mode, SCM, Airplane Mode)**
  > A [custom map](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Use-Custom-Keyboard-Mapping) can be applied to make them working, but some keys like eject button cannot be mapped as they work in a different way than Apple expects

## BIOS Settings

For the most part it's pretty stock, main guys you need to change:

* (Disable CFG Lock)(https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock)
* Change video memory size to 64MB
  > When the video memory is set to factory defaults, the internal display keeps flickering, making it completely unusable.

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
| SSDT-NoHybGfx | N/A | Disables discrete graphics card |
| SSDT-PLUG | N/A | Fixes power management |
| SSDT-PNLF | N/A | Add Backlight control support 

For a full list of ACPI patches, see here: [patches.plist](/ACPI/Custom-SSDTs/patches.plist)

## Kexts

**Hardware specific kexts:**

* [Airportltlwm](https://github.com/OpenIntelWireless/itlwm)
* [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM)
  > Fixes an intermittent bug which Bluetooth appeared to be disabled
* [MSIFanService](https://github.com/lgs3137/MSIFanControl/)
* [VoodooPS2](https://github.com/acidanthera/VoodooPS2)

## Configuration Specifics

**DeviceProperties:**

* **PciRoot(0x0)/Pci(0x1F,0x3)**
  | Key | Type | Value |
  | :--- | :--- | :--- |
  | layout-id  | Number | 2 |
  
* **PciRoot(0x0)/Pci(0x2,0x0)**
  | Key | Type | Value | Comment |
  | :--- | :--- | :--- | :--- |
  | APPL,ig-platform-id | Data | 0700260D | Enables HDMI output |
  | device-id | Data | 12040000 | Fake device for Intel® HD Graphics 4600 Mobile |
  | agdpmod | String | vit9696 | Fixes lag at login screen |
  
**Kernel:**
* **Quirks:**

  `AppleCpuPmCfgLock` set to `False`
    > Only after disabling CFG-Lock
  
**PlatformInfo:**
  * **Generic -> SystemProductName**
  
    `MacBookPro11,4` or `MacBookPro14,1`
    > MacBookPro14,1` is for macOS Ventura
