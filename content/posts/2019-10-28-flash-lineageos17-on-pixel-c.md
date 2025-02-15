---
title: Pixel C 刷 LineageOS 17.0 记录
date: 2019-10-28T15:51:02+08:00
draft: false
---

Pixel C 的官方系统已经停留在8.1不会更新了，目前官方仅仅有一些不定期的安全补丁。好在社区里总会有热心人来延续这类设备的生命周期，在 [XDA][XDA] 上已经有大神更新了 LineageOS 17.0 版本的 ROM ，虽然还存在一些 bug ，但已经堪用了，在此记录一下刷机过程~

## 1. 解锁 bootloader 并刷入自定义 recovery
首先当然是打开设备的开发者选项啦，在设置的关于界面，疯狂连击版本号几次，然后会看到开发者选项已打开。然后进设置的开发者选项，打开 USB 调试模式与 OEM unlocking 选项。

然后可以在设备已解锁状态下，输入命令 `adb reboot bootloader` 进入 fastboot 界面，这时如果 PC 上驱动安装完好，则可以直接刷入 [TWRP][TWRP] 。

这时遇到了一个坑，我在 fastboot 界面输入命令 `fastboot flash recovery xxx.img` ，命令一直返回 `waiting for devices` 。然后意识到是驱动问题，问题是当设备处于 fastboot 界面时在 PC 的设备界面找不到设备，而在设备系统打开的状态下连上 PC 然后更新[驱动][google driver]，从官方下载的驱动也安装不上。最后无可奈何，找不到去哪里找这个驱动，于是安装了一个[360手机助手][360]，然后开机状态下插入设备，坐等驱动安装完成。

驱动装好之后就可以在 fastboot 界面执行 `fastboot flash recovery xxx.img`，然后再执行 `fastboot boot recovery xxx.img` 进入 TWRP。

## 2. 分区清理
进入 TWRP 之后会提示解密 Data 分区，这时输入在系统界面设置的密码是没用的(大坑)，然后我就去 Wipe 界面选择 Data 分区来清理，这时界面一直显示 `formatting data using make_ext4fs`，执行极慢，我甚至以为执行已经挂在这里了，然后我就强制重启(大坑)，重新进入 TWRP ，先更改文件系统为 F2FS ，然后再改回 EXT4 ，然后再执行清理，这时速度倒是快了。然后分别清理 cache, dalvik 和 system 分区。速度比较慢就改下文件系统。

然后问题来了，按顺序刷入 LineageOS 17.0, opengapps, 和 magisk 之后，进入系统会先显示 LineageOS 17.0 的 logo，然后重启设备，然后进入 TWRP ，这时候点击 wipe 界面里面的任何分区都是报错 `unable to mount storage`。

最后，莫得办法，先去找了下官方的[救砖包][factory image]，重新刷入官方系统，然后再来一次刷 TWRP 。这时就老实了， wipe data 的时候安安静静的等待完成。按照网上的说法， wipe 时间从 30 分钟到 8 个小时不等。于是我都打算关灯睡觉了，结果 20 分钟左右的样子一看，竟然已经完成了。

## 3. 卡刷系统
wipe 完了之后又来一坑，以前刷一加 3T 的时候这时候手机连上 PC 就可以直接传文件卡刷了，但是 Pixel C 竟然连上 PC 之后完全找不到 MTP 设备。于是莫得办法，拿出 otg 转 USB 线来，文件传入 U 盘里来刷。这次之后刷入一切顺利~

不过现在 [opengapps][opengapps] 的 10.0 版本还有一个[问题][issue]，system 分区无法 mount 导致刷入时会报错， XDA 上有用户分享说刷入 opengapps 之前先挂载 system 分区即可解决这个问题，不过因为没有找到 10.0 版本的下载地址，暂时未刷入。

至于 [magisk][magisk]，等待 opengapps 10.0 版本发布之后一起刷，没有 Play 商店的 Pixel C 感觉就没那么好用，虽然可以暂时用 apkpure 来代替。

## 最后
当前发现了两个 bug ，屏幕旋转关闭状态时，竖屏锁定状态会被重置为横屏锁定状态，这个比较影响体验，后续尝试报告 bug 试一下。

另一个是在设置了电池图标显示百分比用量时，图标并未改变。还好平时平板一般在家使用，这个问题没啥影响。

[XDA]:https://forum.xda-developers.com/pixel-c/development/rom-t3591152
[TWRP]:https://twrp.me/google/googlepixelc.html
[google driver]:https://developer.android.com/studio/run/win-usb
[360]:https://sj.360.cn/index.html
[factory image]:https://developers.google.com/android/images
[opengapps]:https://opengapps.org/
[issue]:https://github.com/opengapps/opengapps/pull/773
[magisk]:https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445