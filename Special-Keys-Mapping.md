# Special Keys Mapping

To enable special keys such as *Eject, Screen off, ECO mode, SCM, and Airplane mode* to work on macOS, you need to create an SSDT to allow Apple to recognize them. 

In this SSDT, you need to map your PS2 code to the key that Apple understands:

| Key            | PS2 Code | EC Query | Comments |
| -------------- | -------- | -------- | -------- |
| Eject          | e042     | -        |          |
| Screen off     | e036     | -        |          |
| ECO mode       | e02b     | -        |          |
| SCM            | e039     | -        |          |
| Airplane mode  | e076     | _Q84     |          |
| F2             | e06e     | _QB4     |          |
| F4             | e06f     | -        |          |
| F5             | e04e     | -        |          |
| F6             | e06e     | -        |          |
| F10            | -        | -        | Not able to find out the code nor query |
| Volume up      | e030     | -        |          |
| Volume down    | e02e     | -        |          |
| Brightness up  | e077     | _QB8     |          |
| Brightness down| e078     | _QB7     |          |

---

**To achieve this, there are two ways:**

**1. Map PS2 codes to ADB codes:**
   
- Save the following code in a SSDT and and replace "AAAA=BB" with the PS2 codes from the table above with the ADB codes from this [file](https://github.com/acidanthera/VoodooPS2/blob/master/VoodooPS2Keyboard/ApplePS2ToADBMap.h):

```
DefinitionBlock ("", "SSDT", 2, "IVSOAR", "FN", 0x00000000)
{
    External (_SB_.PCI0.LPCB.EC__, DeviceObj)
    External (_SB_.PCI0.LPCB.PS2K, DeviceObj)

    Name (_SB.PCI0.LPCB.PS2K.RMCF, Package (0x02)
    {
        "Keyboard", 
        Package (0x04)
        {
            "Custom ADB Map", 
            Package (0x02)
            {
                Package (0x00){}, 
                "AAAA=BB"
            }
        }
    })
}
```
> In case of using another SSDT which patches another keyboard settings, merge this code into that SSDT


**2. Map EC Queries to PS2 codes:**
   
- Read this [guide](https://www.insanelymac.com/forum/topic/305030-guide-how-to-fix-brightness-hotkeys-in-dsdt-ssdt-hotpatch/), and replace the EC Query from the table above with the PS2 codes from this [file](https://github.com/acidanthera/VoodooPS2/blob/master/VoodooPS2Keyboard/ApplePS2ToADBMap.h).
