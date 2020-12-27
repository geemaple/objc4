# **objc runtime** 
![build_status](https://github.com/0xxd0/objc4/workflows/build/badge.svg) 
[![Join the chat at https://gitter.im/0xxd0/objc4](https://badges.gitter.im/0xxd0/objc4.svg)](https://gitter.im/0xxd0/objc4?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) 
![support](https://img.shields.io/badge/support-macOS%20%7C%20iOS-orange.svg)

This project is a buildable and debuggable version of latest Objective-C runtime (**objc4-787.1**) on [Apple Open Source](https://opensource.apple.com/tarballs/objc4/)

- [Requirement](#Requirement)
- [Installation](#Installation)
- [Usage](#Usage)
- [objc4 tarballs](#target-dependencies-tarballs)
- [Build Phases](#Build-Phases)
- [Build Setting](#build-setting)
- [License](#license)


## **Requirement**
[![Xcode 12](https://img.shields.io/badge/Xcode-12-blue?colorA=1A5DE3&colorB=2A2C3A)](https://developer.apple.com/xcode/) 
[![macOS Catalina](https://img.shields.io/badge/macOS-Catalina-blue?colorA=314C78&colorB=181B2D)](https://developer.apple.com/macos/)

| macOS | Version | objc4 tarball version |
|------|--------| ---- |
| [![macOS Catalina](https://img.shields.io/badge/macOS-Catalina-blue?colorA=314C78&colorB=181B2D)](https://developer.apple.com/macos/) | ![macOS Version](https://img.shields.io/badge/-10.15.4%20~%2010.15.6-181B2D) | [objc4-787.1](https://github.com/0xxd0/objc4/releases/tag/objc4-787.1) |
| [![macOS Catalina](https://img.shields.io/badge/macOS-Catalina-blue?colorA=314C78&colorB=181B2D)](https://developer.apple.com/macos/) | ![macOS Version](https://img.shields.io/badge/-10.15.1-181B2D) | [objc4-781](https://github.com/0xxd0/objc4/releases/tag/objc4-781) | 
| [![macOS High Sierra](https://img.shields.io/badge/macOS-High%20Sierra-blue?colorA=CC4027&colorB=181B2D)](https://developer.apple.com/macos/) | ![macOS Version](https://img.shields.io/badge/-10.13.x-181B2D) | [objc4-723](https://github.com/0xxd0/objc4/releases/tag/objc4-723) | 

## **Installation**

#### Manually
Download zip or clone this repo, select **objc scheme** and build.


## **Usage**
After building the **objc scheme**, manually integrate generated `libobjc.A.dylib` into your project manually. Or you can use **objc-inspect** scheme which is a preset inspector for debugging objc4 runtime.


## **objc4 tarballs**
- [objc4-787.1](https://opensource.apple.com/tarballs/objc4/objc4-787.1.tar.gz)
- [xnu-6153.41.3](https://opensource.apple.com/tarballs/xnu/xnu-6153.41.3.tar.gz)
- [Libc-1353.41.1](https://opensource.apple.com/tarballs/Libc/Libc-1353.41.1.tar.gz)
- [dyld-733.6](https://opensource.apple.com/tarballs/dyld/dyld-733.6.tar.gz)
- [libauto-187.tar.gz](https://opensource.apple.com/tarballs/libauto/libauto-187.tar.gz)
- [libclosure-74](https://opensource.apple.com/tarballs/libclosure/libclosure-74.tar.gz)
- [libdispatch-1173.40.5](https://opensource.apple.com/tarballs/libdispatch/libdispatch-1173.40.5.tar.gz)
- [libplatform-220](https://opensource.apple.com/tarballs/libplatform/libplatform-220.tar.gz)
- [libpthread-416.40.3](https://opensource.apple.com/tarballs/libpthread/libpthread-416.40.3.tar.gz)


## **Build Phases**

#### Private Header 
| objc header | #include | tarball |
|------|--------|---------|
| objc-os.h | `#include <sys/reason.h>` | /xnu-6153.41.3/bsd/sys/reason.h |
| objc-os.h | `#include <mach-o/dyld_priv.h>` | /dyld-733.6/include/mach-o/dyld_priv.h |
| objc-os.h | `#include <os/lock_private.h>` | /libplatform-220/private/os/lock_private.h |
| lock_private.h | `#include <os/base_private.h>` | /libplatform-220/private/os/base_private.h |
| objc-os.h | `#include <System/pthread_machdep.h>` | removed in latest Libc tarball (Libc-1353.41.1), this header should be commented-out |
| pthread_machdep.h | `#include <System/machine/cpu_capabilities.h>` | /xnu-6153.41.3/osfmk/machine/cpu_capabilities.h |
| objc-os.h | `#include <pthread/workqueue_private.h>` | /libpthread-416.40.3/private/workqueue_private.h | 
| objc-os.h | `#include <objc-shared-cache.h>` | /dyld-733.6/include/objc-shared-cache.h | 
| objc-errors.mm | `#include <_simple.h>` | /libplatform-220/private/_simple.h | 
| objc-block-trampolines.mm | `#include <Block_private.h>` | /libclosure-74/Block_private.h |
| objc-os.h | `#include <crt_externs.h>` | /Libc-1353.41.1/include/crt_externs.h |
| objc-runtime-new.mm | `#include <mach/shared_region.h>` | /xnu-6153.41.3/osfmk/mach/shared_region.h |
| objc-cache.mm  | `#include <kern/restartable.h>` | /xnu-6153.41.3/osfmk/mach/restartable.defs, build from xnu kernel |
| objc-os.h | `#include_next <CrashReporterClient.h>` => `#include <CrashReporterClient.h>` | /Libc-825.24/include/CrashReporterClient.h | 
| objc-exception.mm | `#include <objc/objc-abi.h>` | removed |
| objc-gdb.h | `#include <objc/maptable.h>` | removed |

#### Private Header Included Header
| private header | #include | tarball |
|------|--------|---------|
| tsd_private.h | `#include <os/tsd.h>` | /xnu-6153.41.3/libsyscall/os/tsd.h |
| tsd_private.h | `#include <pthread/spinlock_private.h>` | /libpthread-416.40.3/private/spinlock_private.h |
| lock_private.h | `#include <pthread/tsd_private.h>` | /libpthread-416.40.3/private/tsd_private.h |
| workqueue_private.h | `#include <pthread/qos_private.h>` | /llibpthread-416.40.3/private/qos_private.h |
| qos_private.h | `#include <sys/qos_private.h>`  | /libpthread-416.40.3/sys/qos_private.h |

#### Bridge OS

In public macosx sdk (latest Xcode 12.2), bridgeos (e.g. `__has_feature(attribute_availability_bridgeos)`) is unavailable, bridgeos availability should be removed or commented-out.

#### dyld

In latest dyld-733.6 (dyld-421.2 later), apple use this [ruby script](https://opensource.apple.com/source/dyld/dyld-733.6/bin/expand.rb) to expand versions, platfrom versions from a `versionSets` which defined in a YAML file, code generated by this script will be inserted after `@MAC_VERSION_DEFS@`, `@IOS_VERSION_DEFS@`, `@WATCHOS_VERSION_DEFS@`, `@TVOS_VERSION_DEFS@` and `@BRIDGEOS_VERSION_DEFS@` in `dyld_priv.h`. For more detail please refer to [dyld](https://opensource.apple.com/source/dyld).

#### XNU

`<kern/restartable.h>` is generated form `restartable.defs` in xnu tarball during building xun kernel, which is a little different from the one that shipped with public sdk that located in `/Library/Developer/CommandLineTools/SDKs/MacOSX10.15.sdk/System/Library/Frameworks/Kernel.framework/Versions/A/Headers/kern/restartable.h`.


## **Build Setting**
| Declaration | Value |
|-------------|-------|
| `HEADER_SEARCH_PATHS` | $(SRCROOT)/../macosx.internal/System/Library/Frameworks/System.framework/PrivateHeaders, also append `$(inherited)` to target objc |
| `GCC_PREPROCESSOR_DEFINITIONS` | LIBC_NO_LIBCRASHREPORTERCLIENT, also append `$(inherited)` to target objc |
| `ORDER_FILE` | $(SRCROOT)/libobjc.order |
| `OTHER_LDFLAGS[sdk=macosx*]` | -lc++abi -Xlinker -sectalign -Xlinker __DATA -Xlinker __objc_data -Xlinker 0x1000 -Xlinker -interposable_list -Xlinker interposable.txt, remove build setting in target objc |
| `OTHER_LDFLAGS[sdk=iphoneos*][arch=*]` | -lc++abi -Wl,-segalign,0x4000 -Xlinker -sectalign -Xlinker __DATA -Xlinker __objc_data -Xlinker 0x1000 -Xlinker -interposable_list -Xlinker interposable.txt, remove build setting in target objc |
| `OTHER_LDFLAGS[sdk=iphonesimulator*][arch=*]` | -lc++abi -Xlinker -interposable_list -Xlinker interposable.txt, remove build setting in target objc |

### Run Script
Evidently public macosx sdk is our only choice, we need to update value of parameter `-sdk` from `macosx.internal` to `macosx` in run script of objc target. 


## License
This project is released under the **MIT License**. The objc4 project is released under the **APPLE PUBLIC SOURCE LICENSE Version 2.0**.
