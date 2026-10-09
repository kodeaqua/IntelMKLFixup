# !!WARNING!!
Me (Kaitlyn), or Carnations Botanica, and the maintainer of this fork (kodeaqua) are not responsible for any data loss incurred by using this kernel extension. Although it is ***highly*** unlikely, you have been warned.

## IntelMKLFixup
Dead-simple Intel(tm) MKL (Math Kernel Library) patcher for macOS, with a twist.

## Why?
Hackintoshes with AMD CPUs have infamously had a problem with software compiled against Intel's MKL, often resulting in many popular applications just not running correctly or at all.

This is where IntelMKLFixup comes in, IntelMKLFixup will *invisibly* patch bits of Intel's MKL in memory to help provide compatibility for AMD CPUs, without any user interaction or tweaking. Thus, allowing applications that once ran incorrectly or didn't work at all, to now run with little to no issues.

## Requirements
- [Lilu](https://github.com/acidanthera/Lilu/releases)

## Installation
1. Install [Lilu](https://github.com/acidanthera/Lilu/releases) and make sure it loads before plugins.
2. Add `IntelMKLFixup.kext` to `EFI/OC/Kexts` and your `config.plist` (`Kernel -> Add`), after Lilu.
3. Reboot. With `-imklfxdbg` set, check `log show --predicate 'eventMessage contains "imklfx"'` for `Patched _mkl_serv_intel_cpu_true` lines.

## Boot arguments
| Argument | Effect |
| --- | --- |
| `-imklfxoff` | Disable the plugin |
| `-imklfxdbg` | Enable debug logging (Debug builds) |
| `-imklfxbeta` | Allow loading on unsupported (newer) macOS versions |

## Building
Requires Xcode, [MacKernelSDK](https://github.com/acidanthera/MacKernelSDK) and a built `Lilu.kext` in the project root:

    xcodebuild -jobs 1 -configuration Release

## Limitations
- Only the `_mkl_serv_intel_cpu_true` check of modern MKL versions is patched; older MKL versions may need more.
- The patch runs per memory page, so a signature straddling a page boundary is not matched.

## Credits & Thanks
- [vit9696](https://github.com/vit9696) (and contributors) for [RestrictEvents](https://github.com/acidanthera/RestrictEvents), which served as the basis for this project.
- [Tomnic](https://macos86.it/profile/69-tomnic/) for [the original patching guide](https://macos86.it/topic/5489-tutorial-for-patching-binaries-for-amd-hackintosh-compatibility/), which helped point me in the right direction.
- [NyaomiDEV](https://github.com/NyaomiDEV) for [AMDFriend](https://github.com/NyaomiDEV/AMDFriend), which served as inspiration for this project.
- And to anybody who gave me words of encouragement or helped me figure out kernel extension development, thank you.
