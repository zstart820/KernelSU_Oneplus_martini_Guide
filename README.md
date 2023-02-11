[ **简体中文** ](README_zh_cn.md) || **English** 

# KernelSU_Oneplus_martini_Guide
Guide of compile KernelSU for Oneplus 9RT(martini)[MT2110/MT2111]

I have made the [original repository](https://github.com/natsumerinchan/KernelSU_Oneplus_9RT_Action.git) private.

## Warning:warning: :warning: :warning:
- 1.Please backup the offical boot.img and vendor_dlkm.img before you flash!!!You can use [ssut/payload-dumper-go](https://github.com/ssut/payload-dumper-go.git) to extract them from `payload.bin` in ROM.zip.(The ROM package version must as same as your using OS.)
- 2.I am not the author of these kernel, so I will not release any compiled products in this repository, please fork this repository (Sync to your private repository is better) and run workflow by yourself.
- 3."If you are not a kernel author and use someone else's source code to build KernelSU, please use it for your own use only and do not share it with others, it is respectful to the author." --[xiaoleGun/KernelSU_Action](https://github.com/xiaoleGun/KernelSU_Action.git) (I have create a Private repository to compile for me.)

## Support ROMS

| Action Name | Kernel source | Used Branch | Kernel Author | Notes |
|:--:|:--:|:--:|:--:|:--:|
| Pixel Experience | [PixelExperience-Devices/kernel_oneplus_martini](https://github.com/PixelExperience-Devices/kernel_oneplus_martini.git) | thirteen | [inferno0230](https://github.com/inferno0230) | Need flash vendor_dlkm.img.Should use OOS13-based version. |
| Pixel OS Inline | [bheatleyyy/kernel_oplus_sm8350](https://github.com/bheatleyyy/kernel_oplus_sm8350.git) | thirteen | [bheatleyyy](https://github.com/bheatleyyy/kernel_oplus_sm8350.git) | Have been inlined all modules.Needn't flash vendor_dlkm.img.Theoretical support for all AOSP13-based third-party ROMs.Unsupport OOS and ColorOS. |

## How to build
### 1.Github Action
Fork this repository and run workflows by yourselves.
If you can not found Actions tab,please go to `settings`-`actions`-`General`,set Actions permissions as 'Allow all actions and reusable workflows'

![Github Action](https://user-images.githubusercontent.com/64072399/216762170-8cce9b81-7dc1-4e7d-a774-b05f281a9bff.png)

```
Run ncipollo/release-action@v1
Error: Error 403: Resource not accessible by integration
```
If you get this notice when you try to upload to releases,please go to `settings`-`actions`-`General`,set Workflow permissions as 'Read and write permissions'.

### 2.Build on your PC
Put [local_build.sh](https://raw.githubusercontent.com/natsumerinchan/KernelSU_Oneplus_martini_Guide/main/local_build.sh) into the kernel source,modify and run it.

- `export ROM_DLKM=pe_dlkm` (It can be `pe_dlkm` or `pixelos_dlkm`,it depends on which rom you want to build for)

- `export SETUP_KERNELSU=true` (If you don't want to compile KernelSU,please set it as `false`)

- `export REPACK_DLKM=true` (It determines whether you want to repack vendor_dlkm.img)

## Instructions
### 1.How to Install
- 1.Download and install [KernelSU Manager](https://github.com/tiann/KernelSU/actions/workflows/build-manager.yml).Install branches other than `main` is not recommended, because they may not work properly. 
- 2.Download [platform-tools](https://developer.android.com/studio/releases/platform-tools) (Don't install from Ubuntu20.04 source.)
- 3.Reboot to fastbootd mode(not bootloader),flash "vendor_dlkm.img"(If exist.)
```
adb reboot fastboot
fastboot flash vendor_dlkm ./vendor_dlkm.img
```
- 4.Reboot to Recovery mode,flash "Kernel-op9rt-signed.zip"
```
fastboot reboot recovery
adb sideload ./Kernel-op9rt-signed.zip
```

### 2.Notes for Update
You may encounter the fastbootd or recovery mode can not connect to the PC, please enter bootloader mode to reflash the official boot.img
```
adb reboot bootloader
fastboot flash boot ./boot.img
fastboot reboot fastboot
```
Now you can continue to update.

### 3.How to uninstall
Please reflash the offical boot.img and vendor_dlkm.img 
```
adb reboot bootloader
fastboot flash boot ./boot.img
fastboot reboot fastboot
fastboot flash vendor_dlkm ./vendor_dlkm.img
```

### 4.How to sync updates to your private repository
Use Github Desktop to clone your private repository to local, then go to the directory and open a terminal.

```
// Sync mainline branch to the repository

git remote add Celica https://github.com/natsumerinchan/KernelSU_Oneplus_martini_Guide.git //This step is only required for the first time update.

git fetch Celica main
```

```
// Cherry-pick commit from the mainline repository to your private one.

git cherry-pick <commit id>
```

Then use Github Desktop to upload commits to Github

## Credits and Thanks
* [tiann/KernelSU](https://github.com/tiann/KernelSU.git): A Kernel based root solution for Android GKI
* [bheatleyyy/kernel_oplus_sm8350](https://github.com/bheatleyyy/kernel_oplus_sm8350.git): PixelOS kernel for OnePlus 9RT 5G (martini)
* [PixelExperience-Devices/kernel_oneplus_martini](https://github.com/PixelExperience-Devices/kernel_oneplus_martini.git): Pixel Experience kernel for OnePlus 9RT (martini)
* [Gainss/Action_Kernel](https://github.com/Gainss/Action_Kernel.git): Kernel Action template
* [xiaoleGun/KernelSU_action](https://github.com/xiaoleGun/KernelSU_action.git): Action for Non-GKI Kernel has some common and requires knowledge of kernel and Android to be used.
* [osm0sis/AnyKernel3](https://github.com/osm0sis/AnyKernel3.git): Flashable Zip Template for Kernel Releases with Ramdisk Modifications
* [rain2wood/erofs](https://github.com/rain2wood/erofs.git): [vendor_dlkm_repacker](https://github.com/natsumerinchan/vendor_dlkm_repacker.git) is based on it.
* [@Dreamail](https://github.com/Dreamail): This [commit](https://github.com/tiann/KernelSU/commit/bf87b134ded3b81a864db20d8d25d0bfb9e74ebe) fix error for non-GKI kernel
