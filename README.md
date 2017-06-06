# Android_TCP_variants
Evaluation of TCP Variants on Android Platform

### Android TCP congestion control commands:

check current congestion control algorithm:
```
$cat /proc/sys/net/ipv4/tcp_congestion_control
```
check available congestion control algorithms:
```
$cat /proc/sys/net/ipv4/tcp_available_congestion_control
```
### ANDROID SETUP:

**Device:**
Nexus 7 (Wi-Fi) - 2013 model
32GB storage

By default, only **TCP Cubic** and **TCP Reno** are available on Nexus 7.
We need to enable and compile other TCP variants in kernel sources.

### Building Android Kernel:
* https://source.android.com/source/building-kernels
* http://pete.akeo.ie/2013/10/compiling-and-running-your-own-android.html
* https://github.com/pbatard/bootimg-tools.git

### Environment Setup:
```
$export PATH=<path-to-prebuilt-gcc>/arm-eabi-4.6/bin:$PATH
$export ARCH=arm
$export SUBARCH=arm
$export CROSS_COMPILE=arm-eabi-
```
MAKE Commands:
```
$make flo_defconfig
$make
```
We need to create boot.img for flashing on the Android device after building the kernel.
```
$./mkbootimg --base 0 --pagesize 2048 --kernel_offset 0x80208000 --ramdisk_offset 0x82200000 --second_offset 0x81100000 --tags_offset 0x80200100 --cmdline 'console=ttyHSL0,115200,n8 androidboot.hardware=flo user_debug=31 msm_rtb.filter=0x3F ehci-hcd.park=3 vmalloc=340M' --kernel kernel --ramdisk ramdisk.cpio.gz -o boot.img
```
#### Install adb and fastboot:
* http://lifehacker.com/the-easiest-way-to-install-androids-adb-and-fastboot-to-1586992378
* https://developer.android.com/studio/releases/platform-tools.html#download

#### Nexus 7 Factory images:
Download latest factory images of Nexus 7 from below
* https://developers.google.com/android/images

### ROOT Guide (Nexus 7):
* https://forum.xda-developers.com/showthread.php?t=2382051
* https://forum.xda-developers.com/showthread.php?t=2467014

We use CF Auto Root for Rooting the device
**CF Auto Root:**
Download and extract below package
* https://download.chainfire.eu/347/CF-Root/CF-Auto-Root/CF-Auto-Root-flo-razor-nexus7.zip
```
$adb devices
$adb reboot bootloader
$root-windows.bat
<<device gets rooted in around 5 minutes>>
```
Enter ROOT mode:
```
$adb devices
$adb shell
$su
#
```

#### RESET Android Device:
```
$adb devices
$adb reboot bootloader
$fastboot oem unlock
$fastboot -w
$fastboot oem lock
$fastboot reboot
```