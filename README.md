# FForked and adapted from https://github.com/3ig/IdeaPad-S540-15IML-hackintosh, I have kept this updated and made my own twists. 
Change WIFI+BT module from Intel to BCM943602CS which is Genuine Apple part, now everything works as Apple as possible. 

Lenovo IdeaPad S540-15IML (Big Sur 11.1 hackintosh) for bios version CNCN16WW

| Specifications | Details |
|:-: |:-: |
| Processor | Intel Core i5-10510U  |
| Memory | 20gb DDR4 2400MHz (4gb soldiered on board) |
| HDD | 512GB Kingston A2000 NVME |
| Graphics Card | Intel UHD Graphics 630|
| ex. Graphics Card |  NA |
| Monitor | 15.6 inch 1920x1080 |
| Sound Card | Realtek ALC257 |
| Card reader | O2 Card Reader |
| Finger print reader | Goodix |
| Network Card | Intel (replaced with Apple BCM943602CS) |

## Current status:
Working:
- Intel Graphics (HDMI works)
- Sound / Mic 
- Bluetooth / Wifi (with BCM943602CS)
- Keyboard
- Touchpad 
- Card reader
- Camera
- USBs
- Brightness adjustment (with Fn keys)
- Battery managment (7 / 8 hour battery with latest BIOS)
- Power managment
- Sleep / Wake
- Nvram

Not working:
- Fingerprint reader (recognised but can't be used) (DISABLED)
- Micron 2200 NVMe SSD (this is not even working on real Apple hardware)

I must say this machine turned out to be a way better hackintosh than i expected given the new 10gen hardware. It took some time but i got everything working smoothly with great battery life and a really fast pci-e card reader (that was kind of important for me).Performance is on par with with 2019 MacBook Pro 13 inch if not better in some cases. 

## UPDATE: 
I will not update Clover config going forward (it's valid for Catalina 10.15.7 ), but it will stay here in the repo in case someone wants to use Clover.
The OpenCore config expects DVMT to be 64mb and CFG LOCK to be disabled in bios. 

#### Change DVMT and CFG Lock: needs windows
Reference https://github.com/lietxia/XiaoXinAir14IML_2019_hackintosh/wiki/DVMT  
`DVMT`:  
* Area (area): `SaSetup`
* Offset (offset): `0x107`
* `01` to `02`

`CFG LOCK`:  
* Area: `CpuSetup`
* Offset (offset): `0x3E`
* `01` to `00`

###### NOTE: 
If you have a compatable NVMe SSD remove SSDT_NVMe-Pcc.aml from /CLOVER/ACPI/Patched
###### NOTE2: 
Bios version CNCN11WW forces RST storage controller mode on the SATA drive so i added CtlnaAHCIPort.kext to boot my system. Even if you are installed on the nvme drive you need this kext to see and open sata devices in osx.

**Change logs:**

**V33**
OpenCore Update to 0.6.8
Kext Update AppleAPC, Lilu, NVMeFix, VirtualSMC, VoodooInput, WhateverGreen
Update Resources folder
Remove AppleEvent from ProtocolOverrides in all plists.
Change Keyforgetthreshold to 1

**V32**
OpenCore update to 0.6.7
Remove bootstrap folder and efi.
Kext update: AppleALC.Kext, VoodooPS2Controller.kext, WhateverGreen.kext, VirtualSMC kexts. 
Remove KeyMergeThreshold from plists.

**V31**
Add Powersave mode CPUFriendData.kext. Choose which one to implement from 10510u folder.
Currently Running Apple Official power management profile. 
Kext Update: VoodooI2C.kext, VoodooI2CHID to 2.5.6

**V30**
Add beast mode CPUFriendData.kext. Thanks to win1010525 and lietxia.
Add CPUFriend.kext to enable beast mode. CPU Shoots up to 4.9ghz single thread. System responsiveness does improve slightly. Battery life negatively affected. 
Update plist to enable both kexts.

**V29**
YogaSMC ECRW.aml Update

**V28**
Kext Update:YogaSMC updated to 1.4.3
Plist clean up

**V27**
Kext Update: YogaSMC updated to 1.4.2

**V26**
SMBios info cleanup
Hardware UUID is generated based on Serial, SMUUID and MLB.
Consolidated MacBookPro 16,2 SMbios info.
Kext Update: VoodooI2C.Kext


**V25**
Add New Kext YogaSMC.kext for advanced Idealpad Control. Load after VirtualSMC. Enabled.
Add New Kext YogaSMCAlter for YogaSMC Alternative mode. Load after VirtualSMC. Disabled. 
Add new dmg for YogaSMC.
Add SSDT-ECRW.aml for EC access. None functional. Disabled in config.
Add SSDT-RCSM.aml for EC access. None functional. Disabled in config.

**V24**
Add new Kext RestrictEvents
Update Kext
	AppleALC
	Lilu
	VitualSMC
	VoodooPS2Controller
	WhateverGreen
Update OpenCore to 0.6.6
Remove BootProtect from plist, change LauncherOption to Full.
Resources updated using https://github.com/acidanthera/OcBinaryData

OpenCore 0.6.6 has a major change to the boot process: it is no longer a driver, but an application that you can boot directly (if curious, this has to do with Surface firmware failing to boot). Therefore, 0.6.6 removes BootProtect (also commonly known as Bootstrap) and replaces it with LauncherOption in its stead. It's not that different from Bootstrap; the difference is while Bootstrap added a boot entry for OpenCore's loader, as OpenCore doesn't need one anymore, LauncherOption will add a boot entry pointing directly to OpenCore.efi (unless you use a custom launcher with LauncherPath).

So why do you care? Well, this update to 0.6.6 will not automatically remove the old Bootstrap boot entry, so if you want to remove the old Bootstrap entry, dortania (ok fine it was basically me with some contributions from /u/dracoflar) has written a handy guide: Updating Bootstrap in 0.6.6

Oh and if you delete the Bootstrap folder as part of the update and you don't have any other entry to boot into OpenCore, you won't be able to get into OpenCore without bcfg in UEFI Shell/efibootmgr from Linux/copying OC's launcher to EFI/BOOT/BOOTx64.efi from another OS.

https://dortania.github.io/OpenCore-Post-Install/multiboot/bootstrap.html#updating-bootstrap-in-0-6-6

**V23**
Update Kext
	VoodooI2C
	VoodooI2CHID
	WhateverGreen new way to handle backlight, made below changes
Change bootarg gfxonln=1 to -igfxblr
Add enable-backlight-registers-fix, 01000000, as data

**V22**
Update OpenCore to 0.6.5
Update OpenCore Plist to 0.6.5
Remove quirk bootorder
Update kext
	NVMeFix
	VoodooI2c
	Remove VoodooI2CSynapsis
	KEEP WhateverGreen Old version, New version causes random GPU initialisation issue.
Update VoltageShift to 1.25

**V21**
19/12/2020

ACPI Disabled SSDT-PMCR.aml, SSDT-DMAC.aml, SSDT-UIAC.aml, SSDT-XSPI.aml
Kext Replaced ACHI Kext to CtlnaAHCIPort.kext

**V20**
02/12/2020

OpenCore update to 0.6.4
Fixed type in config plists where SSDT-XSPI to SSDT-XSPI.aml
Updated kexts AppleALC, Lilu, VistualSMC, VoodooPS2Controller, whatevergreen
Fix ‘PlayChime’ from type Boolean, to type String (with a value of Auto)  

**V19**
30/11/2020

Implementation of VoltageShift for power control
Add VoltageShift.kext to kext
Add VoltageShift.kext to kernel extensions list
	BundelPath - VoltageShift.kext
	Comment - VoltageShift
	ExecutablePath - Contents/MacOS/VoltageShift
	PlistPath - Contents/Info.plist
Copy VoltageShift folder to /Users/anqisong/voltageshift
Add AppleScript for Info, Power, Turbo functions


**V18**
01/10/2020

Update to OpenCore 0.6.3

Big Sur Disable CSR and SIP
	NVRAM 7C436110-AB2A-4BBB-A880-FE41995C9F82
	csr-active-config
	From E70B0000 to FF0F0000

**V17**
01/09/2020

Switch to OpenCore 0.6.2

Unlocked CFG through in UEFI windows.
	Folder InsydeH2OUVE Run WDFInst.exe
	Admin run H2OUVE-W-GUIX63.exe
	File - Load runtime
	Variable - CpuSetup - 0x3E Change 01 to 00
	Save Shutdown Restart

Adjusted DVMT-Prealloc to 64mb in UEFI through windows. 
	Folder InsydeH2OUVE Run WDFInst.exe
	Admin run H2OUVE-W-GUIX63.exe
	File - Load runtime
	Variable - SaSetup - 0x107 (Vertical 100, Horizontal 07) Change 01 to 02
	Save Shutdown Restart
	
Added config plists for different options. 
	For CFG locked state.
	for CFG Unlocked State. 
	For Stolenmem at 54mb 00006003
	For fbmem at 2048mb 00009000
	For fbmem at 3072 000000C0

**V16**
01/08/2020

Forked and adapted from https://github.com/3ig/IdeaPad-S540-15IML-hackintosh
Change WIFI+BT module from Intel to BCM943602CS Genuine Apple part




