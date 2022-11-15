# OpenCore bootloader EFI for booting macOS

![Screenshot](https://i.imgur.com/K7VkOwX.png)

Specs:
CPU - Xeon E5-2680v2
GPU - Radeon RX 560
MB - Machinist X79
> cringe chinese homeless build

Can be used to boot to macOS Monterey (only 12.6 was tested) on PCs with similar specs.

The problem is that you can't boot macOS 12.0 and newer on CPUs without `MSR_IA32_TSC_ADJUST` (03Bh) instruction support, because CpuTscSync kext (which is required to boot) doesn't support them.

Some workarounds have been applied, such as using [CpuTSCSync chinese modification](https://gitee.com/yaming-network/clover-x79-e5-2670-rx588) and setting random value in `UEFI -> Quirks -> TscSyncTimeout` (lmao). With this, you can boot macOS Monterey with 0-5 reboots before success (depends on how lucky you are), but after booting, it's pretty stable.

Don't forget to set your own SMBIOS, ROM, device path to your ethernet adapter (`DeviceProperties -> Add -> [device path] -> built-in` set to `01`).

Thanks to Andrey Lazarev, as I took [his EFI](https://andrew-lazarev.com/sdm_downloads/opencore-monterey-huanan-2-49b-lga2011-xeon-e5-2680-v2-rx580) (the only EFI I found that boots macOS 12+) as base, and edited for myself a bit.

This EFI can also be used to boot macOS Ventura (13.0 tested), but you need to swap dyld shared cache with the one from Ventura installer for Apple Silicon, as support for non-AVX2.0 Intel CPUs has been removed ([everything explained here](https://github.com/dortania/OpenCore-Legacy-Patcher/issues/998)).
