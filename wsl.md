# Links

docs.microsoft.com:  [wslapi.h header](https://docs.microsoft.com/en-us/windows/win32/api/wslapi/)<br>
docs.microsoft.com:  [Manually download Windows Subsystem for Linux distro packages](https://docs.microsoft.com/en-us/windows/wsl/install-manual)<br>
docs.microsoft.com:  [Enable or Disable Windows Features Using DISM](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/enable-or-disable-windows-features-using-dism)<br>
docs.microsoft.com:  [Example: Getting WMI Data from the Local Computer](https://docs.microsoft.com/en-us/windows/win32/wmisdk/example--getting-wmi-data-from-the-local-computer)<br>
docs.microsoft.com:  [Querying the Status of Optional Features](https://docs.microsoft.com/en-us/windows/win32/wmisdk/querying-the-status-of-optional-features)<br>
docs.microsoft.com:  [Win32_OptionalFeature](https://docs.microsoft.com/en-us/windows/win32/cimwin32prov/win32-optionalfeature) (Windows Management Instrumentation)<br>
github.com/microsoft/WSL#2635:  [No ... way to create an AppX bundle for a Linux distribution from scratch (custom distributions)](https://github.com/microsoft/WSL/issues/2635)<br>
Stack Overflow:  [What's “EXISTS WIN://SYSAPPID” condition in “C:\Program Files\WindowsApps” ACL?](https://stackoverflow.com/questions/63455546/whats-exists-win-sysappid-condition-in-c-program-files-windowsapps-acl)<br>
/r/bashonubuntuonwindows/:  [Using WSL-DistroLauncher - example distro .tar.gz files?](https://www.reddit.com/r/bashonubuntuonwindows/comments/87p7d8/using_wsldistrolauncher_example_distro_targz_files/)<br>
salsa.debian.org/rhaist-guest/WSL:  [WSL Distro Launcher Reference Implementation](https://salsa.debian.org/rhaist-guest/WSL)<br>
salsa.debian.org/rhaist-guest/WSL/[create-targz.sh](https://salsa.debian.org/rhaist-guest/WSL/blob/master/create-targz.sh)<br>
renenyffenegger.ch:  [Windows Subsystem for Linux (WSL)](https://renenyffenegger.ch/notes/Windows/Subsystem-for-Linux/index)<br>

# APIs

### wslapi ([win32](https://docs.microsoft.com/en-us/windows/win32/api/wslapi/), [rust](https://docs.rs/wslapi/))

docs say `api-ms-win-wsl-api-l1-1-0.dll`<br>
`wslapi.dll` is a thing too: `dumpbin /exports "%SystemRoot%\System32\wslapi.dll"`:
```
...
1    0 00001110 WslConfigureDistribution
2    1 00001200 WslGetDistributionConfiguration
3    2 000023A0 WslIsDistributionRegistered
4    3 00002CA0 WslLaunch
5    4 00002DB0 WslLaunchInteractive
6    5 00002480 WslRegisterDistribution
7    6 00002570 WslUnregisterDistribution
...
```
Nice clean `C`ish API.  (Un)register tarballs.



# Command Line Magic

```cmd
:: get your own SID
wmic useraccount where name='%USERNAME%' get sid

:: Check/install WSL 1 (note: dism requires elevation, wmic doesn't)
wmic path Win32_OptionalFeature where name="Microsoft-Windows-Subsystem-Linux" get InstallState
dism /online /enable-feature /featureName:Microsoft-Windows-Subsystem-Linux

:: Upgrade to WSL 2?
dism /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

:: Install Ubuntu
curl -L -o ubuntu-1604.appx https://aka.ms/wsl-ubuntu-1604
Add-AppxPackage ubuntu-1604.appx
:: ...launch appx?...

:: Run stuff
bash --login -c "echo \"why hello there\""
wsl --distribution Ubuntu -- /usr/bin/env bash --login -c "echo \"why hello there\""
```

NOTE:  `wsl` might bypass a lot of shell/env setup by default?  I swear I needed all that `--login` magic and nonsense at one point for basic `${PATH}` setup to work right... or maybe that was necessary to avoid having WSL change my CWD?  I can no longer get it to mistreat me as I remember it historically doing...

# Data Magic

`ubuntu-1604.appx` can be unzipped via 7-zip, and contains `ubuntu1604.exe` (launcher/installer?) and `install.tar.gz` (can probably be fed straight to `WslRegisterDistribution`?)



# Paths

#### C:\Program Files\WindowsApps

* Where all the appx files get unpackaged to?
* Magic account has Read & Execute permissions: `S-1-15-3-1024-3635283841-2530182609-996808640-1887759898-3848208603-3313616867-983405619-2501854204`
* Is this the old user you have to grant permissions to for appx packages to access the regular filesystem?
* Also has magic `Exists WIN://SYSAPPID` condition on Users ACL

#### C:\Program Files\WindowsApps\CanonicalGroupLimited.UbuntuonWindows_1604.2017.922.0_x64__79rhkp1fndgsc\

* Where the ubuntu appx supposedly is... might contain the .tar.gz




# Registry

### Two main registry keys:

`HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\{lower-case-guid}\...`<br>
`HKEY_USERS\...\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\{lower-case-guid}\...`<br>

### Subkey notes:

`DefaultUid == 0`     means root<br>
`Version`             refers to the *original* version?  `wsl --list -v` returns 2 for all versions<br>
`State`               is unrelated to running/stopped state?<br>
`DefaultEnvironment`, `KernelCommandLine`, and `PackageFamilyName` are all optional<br>

### HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\\{74dbd901-db33-4d02-b32d-1ca8c8d4a284}

| Name                  | Type          | Value |
| --------------------- | ------------- | ----- |
| (Default)             | REG_SZ        | (value not set)
| BasePath              | REG_SZ        | `\\?\%USERPROFILE%\AppData\Local\Docker\wsl\data`
| DefaultUid            | REG_DWORD     | `0`
| DistributionName      | REG_SZ        | `docker-desktop-data`
| Flags                 | REG_DWORD     | `0xF`
| State                 | REG_DWORD     | `0x1`
| Version               | REG_DWORD     | `2`

### HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\\{daa507a4-8e03-492b-9e29-0509abcb2009}

| Name                  | Type          | Value |
| --------------------- | ------------- | ----- |
| (Default)             | REG_SZ        | (value not set)
| BasePath              | REG_SZ        | `\\?\%USERPROFILE%\AppData\Local\Docker\wsl\distro`
| DefaultUid            | REG_DWORD     | `0`
| DistributionName      | REG_SZ        | `docker-desktop`
| Flags                 | REG_DWORD     | `0xF`
| State                 | REG_DWORD     | `0x1`
| Version               | REG_DWORD     | `2`

### HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Lxss\\{94352c5f-948c-41e0-9ad6-8249cca15249}

| Name                  | Type          | Value |
| --------------------- | ------------- | ----- |
| (Default)             | REG_SZ        | (value not set)
| BasePath              | REG_SZ        | `%USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState`
| DefaultEnvironment    | REG_MULTI_SZ  | `HOSTTYPE=x86_64`
|                       |               | `LANG=en_US.UTF-8`
|                       |               | `PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games`
|                       |               | `TERM=xterm-256color`
| DefaultUid            | REG_DWORD     | `1000`
| DistributionName      | REG_SZ        | `Ubuntu`
| Flags                 | REG_DWORD     | `0xF`
| KernelCommandLine     | REG_SZ        | `BOOT_IMAGE=/kernel init=/init`
| PackageFamilyName     | REG_SZ        | `CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc`
| State                 | REG_DWORD     | `1`
| Version               | REG_DWORD     | `1`
