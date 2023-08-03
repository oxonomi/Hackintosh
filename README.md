# Triple-boot Hackintosh
![bootloader-screenshot](https://github.com/oxonomi/Hackintosh/blob/main/images/OpenCanopy-bootloader.jpg?raw=true)

'Hackintosh' a non-Apple computer running macOS. <br />
'OpenCore' a custom configured boot-loader which injects data into macOS when booting, effectively tricking macOS to believe it's running on apple hardware. A cleaner, more secure and more involved approach to building a hackintosh compared to tools like Clover or Virtual Machines.

Making an OpenCore Hackintosh is a very complex process, it requires time, patience and a passion for learning and tinkering.
Here's an overview of my configuration but this is not a step-by-step guide. This should be only used for reference alongside the [OpenCore guide](https://dortania.github.io/OpenCore-Install-Guide/)
<br />

## Info

<details open>
<summary><strong>My System</strong></summary>
<br />

- OpenCore:	0.9.1  <br />
- macOS: 	Ventura 13.3.1  <br />
- Windows:	10  <br />
- Linux:		Debian 12

</details>

<details>
<summary><strong>Hardware</strong></summary>
<br />
 
- Mobo:		Gigabtye Z490m Gaming X <br />
- CPU:		Intel i5-10600K <br />
- GPU:		Sapphire RX 5600 XT <br />
- RAM:		Corsair Vengeance 2x16GB 3200 <br />
- Wifi + BT:	Fenvi T919 <br />
- Storage:	Crucial P1 1TB (macOS), Crucial MX500 1TB (Windows), Sandisk Ultra Flair 128GB (Linux) <br />
 <br />

![sys-info](https://github.com/oxonomi/Hackintosh/blob/main/images/info.png?raw=true)

<br />
</details>


<details>
<summary><strong>Tools</strong></summary>
<br />
 
- [MountEFI](https://github.com/corpnewt/MountEFI) - Tool to mount drives EFI partitions  <br />
- [MaciASL](https://github.com/acidanthera/MaciASL) - AML compiler  <br />
- [gfxutil](https://github.com/acidanthera/gfxutil) - Device Properties tool  <br />
- [VDADecoderChecker](https://github.com/cylonbrain/VDADecoderCheck) – determine PlatformId and DeviceID <br />
- [CPUFriend](https://github.com/acidanthera/CPUFriend) - dynamic power management  <br />
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) - generate mac serials  <br />
- [gibMacOS](https://github.com/corpnewt/gibMacOS) - build recovery installer <br />
- [SSDTTime](https://github.com/corpnewt/SSDTTime) - create SSDTs  <br />
- [MountEFI](https://github.com/corpnewt/MountEFI) - mounting usb and drive efi <br />
- [USBMap](https://github.com/corpnewt/USBMap) - mapping ports  <br />
- [ProperTree](https://github.com/corpnewt/ProperTree) - GUI for plist + OC snapshot tool  <br />
- [IORegistry](https://github.com/vulgo/IORegistryExplorer) <br />
- [GPUZ](https://www.techpowerup.com/download/gpu-z/) - getting gpu bios.rom   <br />
- [AtomBIOSReeader](https://github.com/kizwan/ATOMBIOSReader) - reading PowerPlayTable data <br />
- [MorePowerTool](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/) - adjust gpu bios.rom  <br />
- [Red BiosEditor](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/) <br />
- [HxD](https://mh-nexus.de/en/hxd/) - read bios.rom as hex  <br />
- [liquidctl](https://github.com/liquidctl/liquidctl) - controlling RGB <br />

<br />
</details>

<br />

## EFI files

<details>
<summary><strong>Drivers</strong></summary>
<br />
 
- HfsPlus.efi			(HFS file system driver)
- OpenCanopy.efi		(OpenCore plugin bootloader GUI)
- OpenRuntime.efi		(OpenCore plugin OC_FIRMWARE_RUNTIME protocol)

<br />
</details>

<details>
<summary><strong>ACPI </strong></summary>
<br />
 
- SSDT-SBUS-MCHC.aml	(System Management Bus
- SSDT-PLUG.aml		(Better CPU management)
- SSDT-AWAC.aml		(Legacy RTC clock)
- SSDT-EC.ami		(Embedded controller)
- SSDT-UIAC.aml		(USB. Switched to using kext*)
<br />
All created manually via DSDT dump, decompiling, converting to SSDTS and compiling SSDTs	
<br />
<br />
</details>

<details>
<summary><strong>Kexts </strong></summary>
<br />
 
- AppleALC.kext				        (Audio) <br />
- CPUFriendDataProvider.kext  (CPU power management and performance tuning) <br />
- IntelMausi.kext			        (Ethernet) <br />
- Lilu.kext				            (OpenCore, allows kexts to patch) <br />
- NVMeFix.kext				        (Support for non-Apple NVMe drives) <br />
- RadeonSensor.kext			      (Reading GPU temps) <br />
- SMCProcessor.kext			      (CPU temps and power) <br />
- SMCRadeonGPU.kext			      (GPU sensor output) <br />
- SMCSuperIO.kext			        (hardware monitoring) <br />
- USBPorts.kext				        (define USB ports. Replaces SSDT-UIAC) <br />
- VirtualSMC.kext			        (Emulates the System Management Controller) <br />
- WhateverGreen.kext			    (OpenCore, graphic patching) <br />

</details>

<br />

## Config.plist
Follow the OpenCore guide to determine which properties to enable and disable for your build. Use ProperTree to do an OC snapshot to populate based of your OC files.


<details>
<summary><strong>MmioWhitelist > Quirks</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| AvoidRuntimeDefrag:  |  true |
| DevirtualiseMmio:  | true |
| EnableSafeModeSlide:  | true |
| EnableWriteUnprotector:  | false |
| ProtectUefiServices:  | true |
| ProvideCustomSlide:  | true |
| RebuildAppleMemoryMap:  | true |
| ResizeAppleGpuBars: |  -1 |
| SetupVirtualMap:  | false |
| SyncRuntimePermissions: |  true |

<br />
</details>

<a id="fan-control-config"></a>
<details>
<summary><strong>DeviceProperties > Add</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| PciRoot(0x0)/Pci(0x2,0x0) (for headless iGPU)| |
| AAPL,ig-platform-id | AwDFmw== (base64), 0300C89B (hex)|
| device-id | xZsAAA== (base64), c59b0000 (hex) |
| PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0) | |
| Shikigva (for Rx 5600) | 80 |
| PP_PhmSoftPowerPlayTable (for fan control) | [SEE RX 5600 XT fan Control section for data value](#fan-control) |
| PciRoot(0x0)/Pci(0x1F,0x3) (for audio) | | 
| layout-id | AQAAAA== (base64), 1 (dec) | 
| No-hda-gfx | AAAAAA== (base64),  0 (dec)| 

<br />
</details>


<details>
<summary><strong>Kernel > Quirks</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| AppleCpuPmCfgLock: |  false |
| AppleXcpmCfgLock:|  true |
| CustomSMBIOSGuid:|  false |
| DisableIoMapper:|  true |
| DisableLinkeditJettison:|  true |
| DisableRtcChecksum: | false |
| ExtendBTFeatureFlags:|  false |
| LapicKernelPanic: | false |
| LegacyCommpage: | false |
| PanicNoKextDump: | true |
| PowerTimeoutKernelPanic:|  true |
| SetApfsTrimTimeout: | -1 |
| XhciPortLimit: | false | 

<br />
</details>


<details>
<summary><strong>Misc > Boot</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| HideAuxiliary:|  true| 

<br />
</details>


<details>
<summary><strong>Misc > Quirks</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| AppleDebug:|  true| 
| ApplePanic:|  true| 
| DisableWatchDog:|  true| 
| Target:|  83| 

<br />
</details>




<details>
<summary><strong>Misc > Security</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| AllowSetDefault: | true	| 
| BlacklistAppleUpdate:|  true	| 
| ScanPolicy:|  0	| 
| SecureBootModel:|  Default	| 
| Vault:|  Optional| 

<br />
</details>



<details>
<summary><strong>NVRAM > Add</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 (Bootloader background)| | 
| DefaultBackgroundColor| AAAAAA== (base65), 000000(hex), Black| 
| 7C436110-AB2A-4BBB-A880-FE41995C9F82| | 
| boot-args | keepsyms=1 debug=0x100 agdpmod=pikera -wegdbg -liludbg -radnoaudio gfxrst=4| 

<br />
</details>


<details>
<summary><strong>PlatformInfo > Generic</strong></summary>
<br />
 
| key | value |
| ----------------------- | ------------- | 
| SystemProductName | iMac20,1 | 

Note: all other platform info is redacted from the config.plst file. Create your own via GenSMBIOS

</details>

<br />

## BIOS Settings
<details>
<summary><strong>BIOS</strong></summary>
<br />
 
| key | value |
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

<br />
</details>



<details>
<summary><strong>OpenCanopy</strong></summary>
<br />
<i>info to be added*</i> 
</details>

<br />

## Extras
<a id="fan-control"></a>
<details>
<summary><strong>RX 5600 XT fan Control</strong></summary>
<br />
 
By default the Graphics card fan only turns on when temp exceeds 60 degrees, theh turns off again when down to 50 degrees, this is too low for my liking.
To do this we inject new GPU PowerPlayTable data via config.plist. To create this data we need to boot into Windows to dump the current GPU bios to find the current PowerPlayTable data location, create a new bios with our alterations, find the new PowerPlayTable data, copy that into our config.plist.


In Windows:
- Using [GPUZ](https://www.techpowerup.com/download/gpu-z/). Dump the GPU BIOS. Outputs bios.rom
- Using [AtomBIOSReeader](https://github.com/kizwan/ATOMBIOSReader). Note the hexadecimal offset and length of PowerPlayTable. In my instance; Offset: cc90, length: 068a
- Using [MorePowerTool](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/). Load bios.rom. Disable Zero RPM, adjust Stop temp 45, adjust Start temp 50. Output a MPPT file via Write SPPT
- Using [Red BiosEditor](https://www.igorslab.de/en/download-area-new-version-of-morepowertool-mpt-and-final-release-of-redbioseditor-rbe/). Open bios.rom. Load the MPTT file. Save bios.rom (save a new, don’t override original bios.rom)
- Using [HxD](https://mh-nexus.de/en/hxd/), hexadecimal editor. Navigate to the offset value from the ATOMBiosReader, cc90. Copy the data from here with the length, 068a
-	It will look like this:
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
- WARNING: do not just copy and paste my data, even if you have the same card. THIS COULD IRREVERSIBLE BRICK YOUR CARD.
- Insert your PowerPlatTable data into the DeviceProperties [PP_PhmSoftPowerPlayTable](#fan-control-config) in the config.plist
- With the Radeon kexts, you can now monitor your fans behaviour and observe the alterations you made.

<br />
</details>






<details>
<summary><strong>Case RGB and AIO pump speed</strong></summary>
<br />
 
In my build I have a RGB lighting with a NZXT Kraken AIO. We can only control the lighting via GUI in Windows as there's no app for mac. To control in mac I use a CLI tool called [liquidctl](https://github.com/liquidctl/liquidctl). I then created simple AppleScripts to run the Shell scripts at loggin / Script menu in the menu-bar.

example script 1 (static red):
```
do shell script "/usr/local/bin/liquidctl --match kraken set ring color fixed ff0000"
do shell script "/usr/local/bin/liquidctl --match kraken set logo color fixed 000000"
```

example script 2 (fading red-orange):
```
do shell script "/usr/local/bin/liquidctl --match kraken set ring color fading ff0f00 330000 --speed slowest"
do shell script "/usr/local/bin/liquidctl --match kraken set logo color fixed 000000"
```

With the same tool I set my CPU AIO pump speed paramaters:
```
do shell script "/usr/local/bin/liquidctl --match kraken set pump speed 20 20 30 30 31 40 32 43 33 46 35 50 36 60 38 70 40 80 45 90 50 100"
```
<br />
</details>



<details>
<summary><strong>Overclock / Undervolt optimisation</strong></summary>
<br />
<i>info to be added*</i> 
<br />
I have 4 bios profiles for different workloads.<br />
<br />
<strong>General OC - non-AVX workloads: Coding, Browsing, Gaming</strong> <br />
Adaptive (no power/time limits) <br />
5Ghz core, 4.6Ghz uncore, -0.XXv, -300Mhz AVX offset <br />
CinebenchR23 = s: m: <br />
Geekbench = s: m: <br />
<br />
<strong>AVX OC - AVX workloads:  Video Editing</strong> <br />
Adaptive (no power/time limits) <br />
4.8Ghz core, 4.6Ghz uncore, -0.XXv, -0 AVX offset <br />
CinebenchR23 = s: m: <br />
Geekbench = s: m: <br />
max V: <br />
max temp: <br />
<br />
<strong>Fixed Max</strong> <br />
Fixed core <br />
4.9Ghz core, 4.6Ghz uncore, 1.33v, -100Mhz AVX offset <br />
CinebenchR23 = s: m: <br />
Geekbench = s: m: <br />
max V: <br />
max temp: <br />
<br />
<strong>Undervolt</strong><br />
Adaptive <br />
Stock frequencies, -0.1v <br />
CinebenchR23 = s: m: <br />
Geekbench = s: m: <br />
max V: <br />
max temp: <br />
<br />
<strong>Stock</strong><br />
Stock frequencies, -0.1v <br />
CinebenchR23 = s: m: <br />
Geekbench = s: m: <br />
max V: <br />
max temp: <br />

<br />

* Either i've either lost the silcon lottery and have a hot chip, or i need a new AIO (maybe because the v1 hack case design)  
<br />
</details>

<details>
<summary><strong>Benchmarks</strong></summary>
<br />
 
Max OC<br />
![Cinebench](https://github.com/oxonomi/Hackintosh/blob/main/images/Cinebench-OC.png?raw=true)
<br />
![Geekbench5](https://github.com/oxonomi/Hackintosh/blob/main/images/Geekbench5-OC.png?raw=true)
<br />
![Geenbench-opencl](https://github.com/oxonomi/Hackintosh/blob/main/images/Geek%20bench%20compute%20OpenCL.png?raw=true)
![Geekbench-metal](https://github.com/oxonomi/Hackintosh/blob/main/images/Geek%20bench%20compute%20Metal.png?raw=true)
<br />
</details>

