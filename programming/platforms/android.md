# Links

* IDEs/Tools: [Android Studio](https://developer.android.com/studio)
* APIs: [docs.gl](http://docs.gl/),
  [Android API Reference](https://developer.android.com/reference) ([Package Index](https://developer.android.com/reference/packages), [Android NDK API Reference](https://developer.android.com/ndk/reference))
* CI: [AWS Device Farm](https://aws.amazon.com/device-farm/) ([Sample](https://github.com/aws-samples/aws-device-farm-sample-app-for-android)), [Xamarin Test Cloud](https://testcloud.xamarin.com/), [Smartphone Test Farm](https://github.com/DeviceFarmer/stf)

# Useful Commands

## Host commands

| Command | What |
| ------- | ---- |
| `%LOCALAPPDATA%\Android\Sdk\...`      | ADB and other commands are scattered throughout here
| `adb`                                 | ...help text
| `adb devices`                         | Devices
| `adb logcat --clear && adb logcat`    | Device Logs
| `adb install path/to/the.apk`         | Install an APK
| `adb shell`                           | Interactive shell
| `adb shell [command]`                 | Run shell command on target

## Target commands

| Command | What |
| ------- | ---- |
| `adb shell am`                                        | ...help text for Activity Manager
| `adb shell pm`                                        | ...help text for Package Manager
| `adb shell pm list packages`                          | List all installed packages (pipe through `findstr com.maulingmonkey` ?)
| `adb shell pm uninstall com.mm.rust`                  | Fully uninstall package after uninstalling through apps borks the package
| `adb shell pm path com.mm.rust`                       | Check installed path to see if it changed / actually installed
| `adb shell ls -l  pm path com.mm.rust`                | Check installed path to see if it changed / actually installed
| `adb shell ls -l /data/app/com.mm.rust.../base.apk`   | Check size/time
| `adb shell getprop`                                   | See device settings list
| `adb shell getprop ro.product.cpu.abilist`            | Check device architecture
| `adb shell getprop ro.build.version.sdk`              | Check device SDK version
| `adb shell setprop ...`                               | Set device settings

```cmd
:: Screenshot
adb shell screencap -p /sdcard/screen.png
adb       pull         /sdcard/screen.png
adb shell rm           /sdcard/screen.png
```

# Devices

| Device    | [ro.product.cpu.abilist](https://developer.android.com/ndk/guides/abis) | [Android Ver.](https://developer.android.com/studio/releases/platforms) | [ro.build.version.sdk](https://developer.android.com/studio/releases/platforms) | Description |
| --------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ----------- |
| [SM-T510](https://www.samsung.com/ca/tablets/galaxy-tab-a-2019-101/SM-T510NZDAXAC/)   | `armeabi-v7a,armeabi` | 9.0           | 28 | Samsung Galaxy Tab A
| [SM-N900](https://www.sammobile.com/samsung/galaxy-note-3/specs/SM-N900/)             | ???                   | 5.0           | 21 | Samsung Galaxy Note 3

# Workarounds and Android/Samsung/??? Bugs

## App can't be reinstalled after uninstall

Uninstalling through the tablet apps list UI on my `SM-T510` leaves android packages in a partially uninstalled state
where the command line tools can't reinstall to the same package name afterwards, despite supposedly "succeeding" (they
won't show up in the app list nor in `adb shell pm list packages`).  To work around this bug, you can use adb to fully
uninstall with e.g. `adb shell pm uninstall com.maulingmonkey.rust_android_sample`, then reinstall normally.

## I keep ending up with stale builds

Flakey USB connections can cause `adb` to lie and say it successfully installed a package when it didn't.  If you want
to automatically detect this, parse the output of `adb shell pm path com.maulingmonkey.rust_android_sample` before and
after install.  If the path didn't change, it probably didn't redeploy.  You can doublecheck the file size and time
(again before/after install) if you want to eliminate false positives.  If the given path was
`package:/data/app/com.maulingmonkey.rust_android_sample-GmoQAziYbu5PsGZYuXfzUA==/base.apk` , you can then strip the
`package:` prefix and run: `adb shell ls -l /data/app/com.maulingmonkey.rust_android_sample-GmoQAziYbu5PsGZYuXfzUA==/base.apk` .
If that didn't change either, the package almost certainly failed to reinstall.

## My APK is too large (>100MB!)

For the google store, you need to use <a href="https://developer.android.com/google/play/expansion-files">APK Expansion Files</a>.
For the amazon store, larger APKs (1GB?) are supported, and APK Expansion Files are not.

## I can't read my `.obb` expansion files

Earlier versions of the Downloader Library failed to request the proper storage permissions at runtime, although
[apkx_sample](https://github.com/google/play-apk-expansion/blob/9ecf54e5ce7c5a74a2eeedcec4d940ea52b16f0e/apkx_sample/src/com/example/google/play/apkx/SampleDownloaderActivity.java#L446-L514)
looks like it might be requesting everything now?

The worst part is that *sometimes* access would work, sometimes not!

Bug w/ details?: <https://issuetracker.google.com/issues/37075181>
-   `targetSdkVersion=23`+?
-   Android 7.0? "Issue can be reproduced on Samsung Galaxy S8 and S8+. [...] Please confirm, we can reproduce this issue on S8 running 7.0 builds."
-   `*.obb` file is owned by `root` until reboot (at which point it's re-owned by the app-specific user?)
-   `"android.permission.READ_EXTERNAL_STORAGE"` specifically is required for read (`"WRITE_EXTERNAL_STORAGE"` should also be requested to download/update said obb?)

## SIGILL

[A big LITTLE Problem: A Tale of big.LITTLE Gone Wrong](https://medium.com/@niaow/a-big-little-problem-a-tale-of-big-little-gone-wrong-e7778ce744bb) - "LITTLE" cores may not have all the instructions of a "big" core, leading to occasional crashes.

## My program crashes so hard the debugger detatches!

`glLinkProgram` can crash so hard the debugger detatches with some invalid shaders on some phones.
I recall a pixel shader involving array parameters - strictly speaking not legal GLES2, but something that worked on many phones - might've caused this in one case.
Resort to printf style debugging to locate the problem.

# UI Nonsense

* Theme.NoDisplay
* https://commonsware.com/blog/2015/11/02/psa-android-6p0-theme.nodisplay-regression.html
* call `finish()`
