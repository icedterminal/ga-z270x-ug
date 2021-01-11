[OpenCore Guide](https://dortania.github.io/OpenCore-Install-Guide/) | [Version 0.6.3](https://github.com/acidanthera/OpenCorePkg/releases)

Currently two issues are present:

1. CPU idles down to ~1.8GHz. Will not drop to 800MHz.
    - While SSDT plugin works to a degree, I have tried to debug this issue and am unable to. PRgen does not work at all. Literally generates one state. Dropping CPU SSDT can cause boot failures and introduces a different issue.
2. PowerNap, Sleep and Hibernate do not function. System goes for this power state and hangs. Powered on but in a frozen state. Does not respond to input. Screen is black.
    - Since CPU can't idle correctly, these are never expected to work.

This EFI was created using firmware version `F9d`. If you use an older verison, consider upgrading. While in theory it should be fine to use an older version, you never know.

- 6th gen Skylake CPUs will use iMac 17,x.
- 7th gen Kabylake CPUs will use iMac 18,x.

I consider this complete at this point. All else has been worked out. I will keep it updated as needed. With each OC version I will attempt to debug CPU power states.

## Z270 Series

Device | UD3 | UD5 | G5 | G7 | G8 | G9 | GK3 | GK5 | GK7 | UG 
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --
HDMI | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes
DisplayPort | No | Yes | Yes | Yes | Yes | Yes | No | Yes | Yes | No 
ThunderBolt | No | Yes | No | Yes | Yes | Yes | No | No | No | No
Audio | ALC1220 | ALC1220 | ALC1220 | Creative 3D | Creative 3D | Creative 3D | ALC1220 | ACL1220 | ALC1220 | ACL1220
LAN | Intel | Intel | Intel & Killer | Intel & Killer | Intel & Killer | Killer | Killer | Killer | Intel & Killer | Intel
WiFi | No | No | No | No | Killer | Killer | No | No | No | No
I/O Chipset | ITE | ITE | ITE | ITE | ITE | ITE | ITE | ITE | ITE | ITE

- [Z270X UD3](https://www.gigabyte.com/us/Motherboard/GA-Z270X-UD3-rev-10/sp#sp)
- [Z270X UD5](https://www.gigabyte.com/us/Motherboard/GA-Z270X-UD5-rev-10/sp#sp)
- [Z270X Gaming 5](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-5-rev-10/sp#sp)
- [Z270X Gaming 7](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-7-rev-10/sp#sp)
- [Z270X Gaming 8](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-8-rev-10/sp#sp)
- [Z270X Gaming 9](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-9-rev-10/sp#sp)
- [Z270 Gaming K3](https://www.gigabyte.com/us/Motherboard/GA-Z270-Gaming-K3-rev-10/sp#sp)
- [Z270X Gaming K5](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-K5-rev-10/sp#sp)
- [Z270X Gaming K7](https://www.gigabyte.com/us/Motherboard/GA-Z270X-Gaming-K7-rev-10/sp#sp)

Using this EFI as a reference will work for most of the boards in this lineup. You cannot use it as reference for Gaming 7, 8 and 9. Gigabyte tends to design their boards the same way so the firmwares are really similar. However, some of these are not the best choice and will leave you with devices that do not work. Boards with Creative Audio and Killer WiFi should be avoided. Killer LAN is supported.

## CFG Lock

You should [follow the guide](https://dortania.github.io/OpenCore-Desktop-Guide/extras/msr-lock.html) and disable this lock. This is a much cleaner solution than leaving the quirks enabled. I have left them enabled by default so you can boot without a stall. The instructions tell you to use `setup_var`. However, `setup_var2` and `setup_var3` are alternatives if you get an error.

## USB Patching

You can do this one of two ways: Kext or SSDT for OpenCore. Clover is just SSDT. I included the SSDT, kext and an image inside [`post_install`](/post_install/usb) for reference. If you have the exact same number of ports or use the same ports I do, you can use the USB kext. I use one internal USB header (internal at the bottom) for the X62, and one USB 3.0 header (lower right of RAM) to connect two 3.0 ports on the case. The rest of the rear ports fit within the limit and are enabled. If you want to adjust your ports, enable the port limit patch and follow the guides. For Kext and SSDT you can use a tool called [USBMap](https://github.com/corpnewt/USBMap). The [guide](https://dortania.github.io/USB-Map-Guide/intel-mapping/intel.html) favors kext method. To manually create an SSDT [read here.](https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/)

- USBMap-17.kext is for iMac17,x
- USBMap-18.kext is for iMac18,x

If you intend to use the prebuilt kext, you should select the one for your model and rename it to `USBMap.kext` and place it in `/EFI/OC/Kexts`. The USB Port limit patch is enabled. Please disable it if you do not intend to create your own layout.

## NVRAM

Like Clover, OpenCore works best with this board using emulated NVRAM. I gave it a shot with OC, and while it does work, it is unstable. I recommend you do not use native NVRAM. If you use native NVRAM you may experience the following:

- Logout hangs.
- Rebooting results in no input response for F12 or DEL.
- Phantom boot entries.
- Random behavior from firmware.

I have already enabled the emulation in the config. You will need to [follow the instructions](https://dortania.github.io/OpenCore-Desktop-Guide/post-install/nvram.html?h=logouthook) to complete the setup post-install so values can be written on logout.

## Big Sur (11.0)
iMac 17,x and iMac 18,x are supported models.

v0.6.3 is required to boot. At the moment I am not updating because there is a compatibility problem with WhateverGreen in regards to [DRM](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Chart.md#drm-compatibility-on-macos-11). Big Sur has many changes. Any isues you have booting will have to be debugged on your own.

## Debug Branch

This branch has all logging turned on. It is slower to boot. This branch is where I will be keeping all testing done so as to not interfere with the master branch in case I screw up.

---

See WIKI for additional information.
