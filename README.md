# OpenCore running on MSI CX61-2PC

<p align="center">
  <img src="/images/about-this-mac.jpg">
</p>

## Hardware Specs

| Device | Model |
| :--- | :--- |
| CPU | Intel® Core™ i7-4712MQ |
| RAM | 16GB 1600 MHz DDR3 |
| GPU | Intel® HD Graphics 4600 |
| Audio | Realtek AC269 |
| WiFi | Intel® Dual Band Wireless-AC 7260 |
| Ethernet | Qualcomm Atheros© E2200 |

## What's working

* HDMI Output (including Audio Output)
* Speakers and Headphones
* Webcam
* Keyboard (including FN keys) & Touchpad
* Bluetooth and WiFi
* Ethernet port
* Micro SD Card Reader
* CPU Fan Control
* Sleep and Wake


## What's not working

* **NVIDIA© Geforce 820M**
  > Disabled, is not compatible
* **Special buttons (Eject, Screen Off, Turbo Mode, SCM, Airplane Mode)**
  > A [custom map](https://github.com/RehabMan/OS-X-Voodoo-PS2-Controller/wiki/How-to-Use-Custom-Keyboard-Mapping) can be applied to make them working, but some keys like eject button cannot be mapped as they work in a different way than Apple expects

## Not tested

* **VGA Port**

## BIOS Settings

### Change the graphic aperture size to 64 MB

 1. [Unlock CFG](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html)
   
 2. After booting up in [Modified GRUB Shell](https://github.com/datasone/grub-mod-setup_var), run the following command:
    ```
    setup_var_cv Setup 0x57 0x01 0x00
    ```
    
3. And, finally, write this command:
    ```
    setup_var_cv Setup 0x2B6 0x01 0x02
    ```

## ACPI

### Required SSDTs

| SSDT | ACPI Patches | Comments |
| :--- | :--- | :--- |
| SSDT-FANQ | N/A | CPU Fan Control (Used alongside MSI Fan Service kext) |
| SSDT-FN | N/A | Maps brightness keys to F14 & F15 |
| SSDT-GPRW | `GPRW` to `XPRW`| Fixes wake up from sleeping |
| SSDT-NoHybGfx | N/A | Disables discrete graphics card |
| SSDT-PLUG | N/A | Fixes power management |
| SSDT-PNLF | N/A | Add backlight control support 

## Kexts

### Hardware specific kexts

| Name | Comment |
| :--- | :--- |
| [Airportltlwm](https://github.com/OpenIntelWireless/itlwm)| Enables WiFi |
| [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM) | Enables Bluetooth (Airportltlwm needed) |
| [MSIFanService](https://github.com/ivansoriarab/Compiled-MSI-Fan-Service) | CPU Fan Control (Used alongside MSI EC Control / MSI Fan Control app) |
| [VoodooPS2](https://github.com/acidanthera/VoodooPS2) | Enables Keyboard and Touchpad |

## Configuration Specifics

### DeviceProperties

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
  
### Kernel
* **Quirks:**

  `AppleCpuPmCfgLock` set to `False`
    > Only after disabling CFG-Lock
  
**PlatformInfo:**
  * **Generic -> SystemProductName**
  
    `MacBookPro11,4` or `MacBookPro14,1`
    > `MacBookPro14,1` is for macOS Ventura
    
## Miscellaneous Fixes

### Graphics hardware acceleration in macOS Ventura

1. Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher)
2. Run it and apply **Post Install Volume Patches**
