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

* HDMI output (including audio output)
* Speakers and headphones
* Webcam
* Keyboard (including FN keys) & touchpad
* Bluetooth and WiFi
* Ethernet port
* Micro SD card reader
* CPU fan control
* Wake and sleep


## What's not working

* **NVIDIA© Geforce 820M**
  > Disabled, is not compatible
* **Special buttons (Eject, Screen off, Turbo mode, SCM, Airplane mode)**
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
    
3. Finally, write this command:
    ```
    setup_var_cv Setup 0x2B6 0x01 0x02
    ```

## ACPI

### Required SSDTs

| SSDT |  Comments |
| :--- | :--- |
| [SSDT-FANQ](https://github.com/ivansoriarab/Compiled-MSI-Fan-Control/blob/main/SSDT-FANQ.aml) | CPU fan control<br>*Decompiled SSDT available [here](https://github.com/lgs3137/MSIFanControl/blob/master/SSDT-FANQ.dsl)* |
| [SSDT-FN](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Compiled/SSDT-FN.aml) | Maps brightness keys to F14 & F15 |
| [SSDT-GPRW](https://github.com/dortania/OpenCore-Post-Install/blob/master/extra-files/SSDT-GPRW.aml) | Fixes wake up from sleeping |
| [SSDT-HPET](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Compiled/SSDT-HPET.aml) | Fixes IRQ conflicts (the following [patches](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/patches.plist) must be applied in OpenCore)<br> *Decompiled SSDT available [here](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Decompiled/SSDT-HPET.dsl)* |
| [SSDT-NoHybGfx](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Compiled/SSDT-NoHybGfx.aml) | Disables discrete graphics card<br>*Decompiled SSDT available [here](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/decompiled/SSDT-NoHybGfx.dsl.zip)* |
| [SSDT-PLUG](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Compiled/SSDT-PLUG.aml) | Fixes power management<br>*Decompiled SSDT available [here](https://github.com/ivansoriarab/OpenCore-MSI-CX61-2PC/blob/master/ACPI/Custom-SSDTs/Decompiled/SSDT-PLUG.dsl)*|
| [SSDT-PNLF](https://github.com/dortania/Getting-Started-With-ACPI/blob/master/extra-files/compiled/SSDT-PNLF.aml) | Adds backlight control support 

## Kexts

### Hardware specific kexts

| Name | Comment |
| :--- | :--- |
| [Airportltlwm](https://github.com/OpenIntelWireless/itlwm/releases)| Enables WiFi |
| [BlueToolFixup](https://github.com/acidanthera/BrcmPatchRAM/releases) | Enables Bluetooth (Airportltlwm needed) |
| [MSIFanService](https://github.com/ivansoriarab/Compiled-MSI-Fan-Service/releases) | CPU fan control<br>Used alongside [MSI EC Control / MSI Fan Control](https://github.com/ivansoriarab/Compiled-MSI-Fan-Control/releases) |
| [VoodooPS2](https://github.com/acidanthera/VoodooPS2/releases) | Enables Keyboard and Touchpad |

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

1. Download [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)
2. Run it and apply **Post Install Volume Patches**
