## WARNING
This is not a step-by-step guide. This should be only used for reference alongside the [OpenCore guide](https://dortania.github.io/OpenCore-Install-Guide/)


# Hackintosh
OpenCore:	0.9.1
macOS: 	Ventura 13.3.1
Windows:	10
Linux:		Debian

## Hardware	
- Mobo:		Gigabtye Z490m Gaming X
- CPU:		Intel i5-10600k
- GPU:		Sapphire RX 5600 XT
- RAM:		Corsair Vengeance 2x16GB 3200
- Wifi + BT:	Fenvi T919
- Storage:	Crucial P1 1TB (macOS), Crucial MX500 1TB (Windows), Crucial MX500 1TB (Linux)



![alt text](https://github.com/oxonomi/Hackintosh/blob/main/images/info.png?raw=true)

![alt text](https://github.com/oxonomi/Hackintosh/blob/main/images/Cinebench.png?raw=true)

![alt text](https://github.com/oxonomi/Hackintosh/blob/main/images/Geekbench.png?raw=true)
![alt text](https://github.com/oxonomi/Hackintosh/blob/main/images/Geek%20bench%20compute%20OpenCL.png?raw=true)
![alt text](https://github.com/oxonomi/Hackintosh/blob/main/images/Geek%20bench%20compute%20Metal.png?raw=true)

## Tools
- [MountEFI](https://github.com/corpnewt/MountEFI)
- [MaciASL](https://github.com/acidanthera/MaciASL) - AML compiler
- [gfxutil](https://github.com/acidanthera/gfxutil) - Device Properties tool 
- [VDADecoderChecker](https://github.com/cylonbrain/VDADecoderCheck) – determine PlatformId and DeviceID
- [CPUFriend](https://github.com/acidanthera/CPUFriend) - dynamic power management 
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) - generate mac serials 
- [gibMacOS](https://github.com/corpnewt/gibMacOS) - build recovery installer
- [SSDTTime](https://github.com/corpnewt/SSDTTime) - create SSDTs 
- [MountEFI](https://github.com/corpnewt/MountEFI) - mounting usb and drive efi
- [USBMap](https://github.com/corpnewt/USBMap) - mapping ports 
- [ProperTree](https://github.com/corpnewt/ProperTree) - GUI for plist + OC snapshot tool 
- [IORegistry](https://github.com/vulgo/IORegistryExplorer)
- [GPUZ](https://www.techpowerup.com/download/gpu-z/) - getting gpu bios.rom  
- [AtomBIOSReeader]![image](https://github.com/oxonomi/Hackintosh/assets/130058100/23b2711a-7b3d-4f79-b222-cb7a15d4307c)
- [MorePowerTool](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/) - adjust gpu bios.rom 
- [Red BiosEditor](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/)
- [HxD](https://mh-nexus.de/en/hxd/) - read bios.rom as hex 
- [Liquidctl](https://mh-nexus.de/en/hxd/) - controlling RGB 


## BIOS Settings
| -------------------- |-------------|
| Fast Boot |			          Disable |
| Secure Boot | 		        Disable |
| Serial/COM Port		 |      Disable |
| VT-d 			          |     Disable |
| CSM 			         |      Disable |
| Intel SGX 			    |     Disable |
| Intel Platform Trust	|   Disable |
| Resizable BAR		 |        Disable |
| VT-x			        |       Enable |
| Above 4G decoding	  |     Enable |
| Hyper-Threading		  |     Enable |
| Execute Disable Bit	  |   Enable |
| EHCI/XHCI Hand-off	 |    Enable |
| OS type 			      |     Windows 10 UEFI Mode |
| DVMT Pre-Allocated	 |    64MB |
| SATA Mode 		      |     AHCI |
| XMP               	| 		Profile 1 |


## EFI files

### Drivers
- HfsPlus.efi			(HFS file system driver)
- OpenCanopy.efi		(OpenCore plugin bootloader GUI)
- OpenRuntime.efi		(OpenCore plugin OC_FIRMWARE_RUNTIME protocol)

### ACPI 
- SSDT-SBUS-MCHC.aml	(System Management Bus)
- SSDT-PLUG.aml		(Better CPU management)
- SSDT-AWAC.aml		(Legacy RTC clock)
- SSDT-EC.ami		(Embedded controller)
- SSDT-UIAC.aml		(USB. Switched to using kext*)

All done manually via DSDT dump, decompiling, converting to SSDTS and compiling SSDTs	

### Kexts
- AppleALC.kext				        (Audio)
- CPUFriendDataProvider.kext  (CPU power management and performance tuning)
- IntelMausi.kext			        (Ethernet)
- Lilu.kext				            (OpenCore, allows kexts to patch)
- NVMeFix.kext				        (Support for non-Apple NVMe drives)
- RadeonSensor.kext			      (Reading GPU temps)
- SMCProcessor.kext			      (CPU temps and power)
- SMCRadeonGPU.kext			      (GPU sensor output)
- SMCSuperIO.kext			        (hardware monitoring)
- USBPorts.kext				        (define USB ports. Replaces SSDT-UIAC)
- VirtualSMC.kext			        (Emulates the System Management Controller)
- WhateverGreen.kext			    (OpenCore, graphic patching)
