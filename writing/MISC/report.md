电脑无端重启

panic(cpu 0 caller 0xffffff801ae652fa): Kernel trap at 0xffffff7f9ca8fc27, type 14=page fault, registers:
CR0: 0x0000000080010033, CR2: 0x0000000000000008, CR3: 0x00000004586fd002, CR4: 0x00000000001626e0
RAX: 0x0000000000000000, RBX: 0xffffff804f3df040, RCX: 0x00000000000080fe, RDX: 0x77de4545adff6f04
RSP: 0xffffff922c93b810, RBP: 0xffffff922c93b880, RSI: 0x0000000000000000, RDI: 0x0000000000000000
R8:  0x0000000000000000, R9:  0xffffff803be3c6a8, R10: 0x0000000000000010, R11: 0xffffff8038a2d000
R12: 0x0000000000000000, R13: 0xffffff8065302ea0, R14: 0x0000000000000000, R15: 0xffffff922c93b820
RFL: 0x0000000000010246, RIP: 0xffffff7f9ca8fc27, CS:  0x0000000000000008, SS:  0x0000000000000010
Fault CR2: 0x0000000000000008, Error code: 0x0000000000000002, Fault CPU: 0x0, PL: 0, VF: 0

Backtrace (CPU 0), Frame : Return Address
0xffffff922c93b270 : 0xffffff801ad3bb2b 
0xffffff922c93b2c0 : 0xffffff801ae734d5 
0xffffff922c93b300 : 0xffffff801ae64f4e 
0xffffff922c93b350 : 0xffffff801ace2a40 
0xffffff922c93b370 : 0xffffff801ad3b217 
0xffffff922c93b470 : 0xffffff801ad3b5fb 
0xffffff922c93b4c0 : 0xffffff801b4d2aa9 
0xffffff922c93b530 : 0xffffff801ae652fa 
0xffffff922c93b6b0 : 0xffffff801ae64ff8 
0xffffff922c93b700 : 0xffffff801ace2a40 
0xffffff922c93b720 : 0xffffff7f9ca8fc27 
0xffffff922c93b880 : 0xffffff7f9ca8f0b3 
0xffffff922c93b8c0 : 0xffffff7f9ca8e861 
0xffffff922c93b900 : 0xffffff7f9ca8e67f 
0xffffff922c93b940 : 0xffffff7f9ca8e3bf 
0xffffff922c93ba40 : 0xffffff801b442578 
0xffffff922c93baa0 : 0xffffff7f9ca8d6be 
0xffffff922c93bac0 : 0xffffff801b469270 
0xffffff922c93bb10 : 0xffffff801b4674f1 
0xffffff922c93bb60 : 0xffffff801b470443 
0xffffff922c93bca0 : 0xffffff801ae22d12 
0xffffff922c93bdb0 : 0xffffff801ad419d8 
0xffffff922c93be10 : 0xffffff801ad18635 
0xffffff922c93be70 : 0xffffff801ad2f0e5 
0xffffff922c93bf00 : 0xffffff801ae4b575 
0xffffff922c93bfa0 : 0xffffff801ace3226 
      Kernel Extensions in backtrace:
         com.apple.driver.mDNSOffloadUserClient(1.0.1b8)[2C5E21BB-E8AE-33F7-976D-18CBF7A66D48]@0xffffff7f9ca8d000->0xffffff7f9ca93fff
            dependency: com.apple.iokit.IONetworkingFamily(3.4)[DADDF78F-DD4E-359E-AE63-446D90F3ADDA]@0xffffff7f9b665000

BSD process name corresponding to current thread: mDNSResponder

Mac OS version:
19D76

Kernel version:
Darwin Kernel Version 19.3.0: Thu Jan  9 20:58:23 PST 2020; root:xnu-6153.81.5~1/RELEASE_X86_64
Kernel UUID: A8DDE75C-CD97-3C37-B35D-1070CC50D2CE
Kernel slide:     0x000000001aa00000
Kernel text base: 0xffffff801ac00000
__HIB  text base: 0xffffff801ab00000
System model name: MacBookPro11,4 (Mac-06F11FD93F0323C5)
System shutdown begun: NO
Panic diags file available: YES (0x0)

System uptime in nanoseconds: 134227834413626
last loaded kext at 130778530488735: >!UAudio	320.49 (addr 0xffffff7f9e747000, size 434176)
last unloaded kext at 78862290130859: >usb.cdc	5.0.0 (addr 0xffffff7f9e740000, size 28672)
loaded kexts:
com.tuxera.filesystems.tuxera_ntfs	2017.12.18
ch.tripmode.TripModeNKE	2.0.0
net.nutstore.kext.fsmonk	0000.00.09
com.intel.kext.intelhaxm	6.0.5
net.sf.tuntaposx.tun	1.0
net.sf.tuntaposx.tap	1.0
com.Apowersoft.driver.AudioDevice	1.6.7
@filesystems.smbfs	3.4.1
@fileutil	20.036.15
@filesystems.autofs	3.0
|IO!BSerialManager	7.0.3f5
>AGPM	111.4.2
>!APlatformEnabler	2.7.0d0
>X86PlatformShim	1.0.0
>!AHDA	283.15
>!AUpstreamUserClient	3.6.8
>AudioAUUC	1.70
>!AGraphicsDevicePolicy	4.7.2
>@AGDCPluginDisplayMetrics	4.7.2
>!AHV	1
>|IOUserEthernet	1.0.1
>!A!IHD5000Graphics	14.0.4
>eficheck	1
>pmtelemetry	1
>@Dont_Steal_Mac_OS_X	7.0.0
>!ACameraInterface	7.6.0
>!ALPC	3.1
>!AThunderboltIP	3.1.3
>|Broadcom!B20703USBTransport	7.0.3f5
>!A!ISlowAdaptiveClocking	4.0.0
>!ABacklight	180.1
>!ASMCLMU	212
>!A!IFramebufferAzul	14.0.4
>!AMCCSControl	1.13
>!ATopCaseHIDEventDriver	3430.1
>!UTopCaseDriver	3430.1
>!UCardReader	489.80.2
>@filesystems.apfs	1412.81.1
>AirPort.BrcmNIC	1400.1.1
>!AAHCIPort	341.0.2
>!AVirtIO	1.0
>@filesystems.hfs.kext	522.0.9
>@!AFSCompression.!AFSCompressionTypeDataless	1.0.0d1
>@BootCache	40
>@!AFSCompression.!AFSCompressionTypeZlib	1.0.0
>@private.KextAudit	1.0
>!ASmartBatteryManager	161.0.0
>!AACPIButtons	6.1
>!AHPET	1.8
>!ARTC	2.0
>!ASMBIOS	2.1
>!AACPIEC	6.1
>!AAPIC	1.7
>$!AImage4	1
>@nke.applicationfirewall	303
>$TMSafetyNet	8
>@!ASystemPolicy	2.0.0
>|EndpointSecurity	1
>!UAudio	320.49
>usb.cdc	5.0.0
>@kext.triggers	1.0
>DspFuncLib	283.15
>@kext.OSvKernDSPLib	529
>|IOAVB!F	800.17
>!ASSE	1.0
>@!AGPUWrangler	4.7.2
>!AGraphicsControl	4.7.2
>|Broadcom!BHost!CUSBTransport	7.0.3f5
>|IO!BHost!CUSBTransport	7.0.3f5
>|IO!BHost!CTransport	7.0.3f5
>!AHDA!C	283.15
>|IOHDA!F	283.15
>|IOSlowAdaptiveClocking!F	1.0.0
>!ABacklightExpert	1.1.0
>|IONDRVSupport	569.4
>X86PlatformPlugin	1.0.0
>IOPlatformPlugin!F	6.0.0d8
>@!AGraphicsDeviceControl	4.7.2
>|IOAccelerator!F2	438.3.1
>!ASMBus!C	1.0.18d1
>|IOGraphics!F	569.4
>@plugin.IOgPTPPlugin	810.1
>|IOEthernetAVB!C	1.1.0
>!AHS!BDriver	3430.1
>IO!BHIDDriver	7.0.3f5
>|IO!B!F	7.0.3f5
>|IO!BPacketLogger	7.0.3f5
>!AActuatorDriver	3430.1
>!AMultitouchDriver	3430.1
>!AInputDeviceSupport	3430.1
>!AHIDKeyboard	209
>usb.IOUSBHostHIDDevice	1.2
>usb.networking	5.0.0
>usb.!UHostCompositeDevice	1.2
>!AThunderboltDPInAdapter	6.2.5
>!AThunderboltDPAdapter!F	6.2.5
>!AThunderboltPCIDownAdapter	2.5.4
>!AThunderboltNHI	5.8.6
>|IOThunderbolt!F	7.6.0
>|IOAHCIBlock!S	316.80.1
>|IO80211!F	1200.12.2b1
>mDNSOffloadUserClient	1.0.1b8
>corecapture	1.0.4
>|IOSkywalk!F	1
>usb.!UXHCIPCI	1.2
>usb.!UXHCI	1.2
>|IOAHCI!F	290.0.1
>|IOAudio!F	300.2
>@vecLib.kext	1.2.0
>|IOSerial!F	11
>|IOSurface	269.6
>@filesystems.hfs.encodings.kext	1
>usb.!UHostPacketFilter	1.0
>|IOUSB!F	900.4.2
>!AEFINVRAM	2.1
>!AEFIRuntime	2.1
>|IOSMBus!F	1.1
>|IOHID!F	2.0.0
>$quarantine	4
>$sandbox	300.0
>@kext.!AMatch	1.0.0d1
>DiskImages	493.0.0
>!AFDEKeyStore	28.30
>!AEffaceable!S	1.0
>!AKeyStore	2
>!UTDM	489.80.2
>|IOSCSIBlockCommandsDevice	422.0.2
>!ACredentialManager	1.0
>KernelRelayHost	1
>!ASEPManager	1.0.1
>IOSlaveProcessor	1
>|IOUSBMass!SDriver	157.40.7
>|IOSCSIArchitectureModel!F	422.0.2
>|IO!S!F	2.1
>|IOUSBHost!F	1.2
>!UHostMergeProperties	1.2
>usb.!UCommon	1.0
>!ABusPower!C	1.0
>|CoreAnalytics!F	1
>!AMobileFileIntegrity	1.0.5
>@kext.CoreTrust	1
>|IOTimeSync!F	810.1
>|IONetworking!F	3.4
>|IOReport!F	47
>!AACPIPlatform	6.1
>!ASMC	3.1.9
>watchdog	1
>|IOPCI!F	2.9
>|IOACPI!F	1.4
>@kec.pthread	1
>@kec.corecrypto	1.0
>@kec.Libm	1