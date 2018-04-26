## Linux环境下构建Android的droidVncServer

### 部署NDK
```
1、下载ndk的linux版本安装包 android-ndk-r15c-linux-x86_64.zip

2、直接解压到自己规划好的目录中 ~/Local/
   mkdir ~/Local (如果不存在，请先创建)

   unzip android-ndk-r15c-linux-x86_64.zip -d ~/Local/


3、配置环境变量
   export NDKROOT=~/Local/android-ndk-r15c
   export PATH=$NDKROOT:$PATH

```

### 编译工程
```
ndk-build

```


### 编译工程修改的问题
```
Aborting (set APP_ALLOW_MISSING_DEPS=true to allow missing dependencies)

vim jni/Application.mk

APP_ALLOW_MISSING_DEPS:=true
```

```
jni/jpeg/jidctfst.S:17:10: fatal error: 'machine/cpu-features.h' file not found
vim ./jni/jpeg/jidctfst.S

改行注释掉
// #include <machine/cpu-features.h>

```

```
[armeabi] Compile arm    : jpeg <= jidctfst.S
jni/jpeg/jidctfst.S: Assembler messages:
jni/jpeg/jidctfst.S:66: Error: missing ')'
jni/jpeg/jidctfst.S:66: Error: garbage following instruction -- `pld (r2,#0)'
jni/jpeg/jidctfst.S:259: Error: missing ')'
jni/jpeg/jidctfst.S:259: Error: garbage following instruction -- `pld (sp,#32)'
jni/jpeg/jidctfst.S:271: Error: missing ')'
jni/jpeg/jidctfst.S:271: Error: garbage following instruction -- `pld (ip,#32)'

66， 259， 271行 () 修改成 []

and also set
ANDROID_JPEG_NO_ASSEMBLER:=true
in /jni/jpeg/Android.mk
```

```
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:464: error: undefined reference to 'jpeg_std_error'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:474: error: undefined reference to 'jpeg_CreateCompress'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:602: error: undefined reference to 'jpeg_abort_compress'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:183: error: undefined reference to 'jpeg_set_defaults'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:186: error: undefined reference to 'jpeg_set_quality'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:191: error: undefined reference to 'jpeg_set_colorspace'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:584: error: undefined reference to 'jpeg_start_compress'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:594: error: undefined reference to 'jpeg_write_scanlines'
jni/vnc/./LibVNCServer-0.9.9/common/turbojpeg.c:597: error: undefined reference to 'jpeg_finish_compress'

vim jni/jpeg/Android.mk
删除第一个 if 和 最后的 endif
```


The droid-VNC-server projects consists in three main modules parts: the daemon, wrapper libs and the GUI.

- Daemon -
Provides the vnc server functionality, injects input/touch events, clipboard management, etc
Available in jni/ folder

- Wrapper libs -
Compiled against the AOSP so everyone can build the daemon/GUI without having to fetch +2GB files.
Currently there are 2 wrappers, gralloc and flinger.

Available in nativeMethods/ folder, and precompiled libs in nativeMethods/lib/

- GUI -
GUI handles user-friendly control.
Connects to the daemon using local IPC.

-------------- Compile C daemon ---------------------
On project folder:
  $ ndk-build
  $ ./updateExecsAndLibs.sh

-------------- Compile Wrapper libs -----------------
  $ cd <aosp_folder>
  $ . build/envsetup.sh
  $ lunch
  $ ln -s <droid-vnc-folder>/nativeMethods/ external/

To build:
  $ cd external/nativeMethods
  $ mm .
  $ cd <droid-vnc-folder>
  $ ./updateExecsAndLibs.sh

-------------- Compile GUI------- -------------------
Import using eclipse as a regular Android project
