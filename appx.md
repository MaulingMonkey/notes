# File Format

* Code-signed .zip folders?
* Openable with 7-zip



# Shell

[Appx Module](https://docs.microsoft.com/en-us/powershell/module/appx/?view=win10-ps) (powershell)
* `Get-AppxPackage -Name *Ubuntu*`
* `Add-AppxPackage -Path some.appx`

[MakeAppx.exe](https://docs.microsoft.com/en-us/windows/msix/package/create-app-package-with-makeappx-tool) (any CLI)



# API

* [Windows::Management::Deployment](https://docs.microsoft.com/en-us/uwp/api/windows.management.deployment?view=winrt-19041) (WinRT)



# Registry Nonsense

*   `HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppModel\Repository`
    *   `Families\{PackageFamilyName}\{PackageFullName}`
        | Name          | Type          | Data |
        | ------------- | ------------- | ---- |
        | `Flags`       | `REG_DWORD`   | `4`
        | `InstallTime` | `REG_QWORD`   | `0x1d6b0dd3570c0c7`


*   `HKEY_CLASSES_ROOT\Local Settings\Software\Microsoft\Windows\CurrentVersion\AppModel\Repository`
    * `Families`
        * `CanonicalGroupLimited.Ubuntu16.04onWindows_79rhkp1fndgsc`<br>
            ...
        * `CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc`<br>
            ...
        * `CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc`
            *   `CanonicalGroupLimited.Ubuntu20.04onWindows_2004.2020.812.0_neutral_split.scale-100_79rhkp1fndgsc`
            *   `CanonicalGroupLimited.Ubuntu20.04onWindows_2004.2020.812.0_x64__79rhkp1fndgsc`
        * `CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc`
            *   `CanonicalGroupLimited.UbuntuonWindows_2004.2020.812.0_neutral_split.scale-100_79rhkp1fndgsc`
                * `Flags=1`
            *   `CanonicalGroupLimited.UbuntuonWindows_2004.2020.812.0_x64__79rhkp1fndgsc`
                * `Flags=4`
    * `Packages`
        *   `CanonicalGroupLimited.Ubuntu16.04onWindows_1604.2020.824.0_x64__79rhkp1fndgsc`<br>
            ...
        *   `CanonicalGroupLimited.Ubuntu18.04onWindows_1804.2020.824.0_x64__79rhkp1fndgsc`<br>
            ...
        *   `CanonicalGroupLimited.Ubuntu20.04onWindows_2004.2020.812.0_x64__79rhkp1fndgsc`<br>
            ...
        *   `CanonicalGroupLimited.UbuntuonWindows_2004.2020.812.0_x64__79rhkp1fndgsc`
            | Name                  | Type          | Data          |
            | --------------------- | ------------- | ------------- |
            | `CapabilityCount`     | `REG_DWORD`   | `1`
            | `CapabilitySids`      | `REG_BINARY`  | ...
            | `DisplayName`         | `REG_SZ`      | `Ubuntu`
            | `OSMaxVersionTested`  | `REG_QWORD`   | `0xa00003f700000`
            | `OSMinVersion`        | `REG_QWORD`   | `0xa00003f6d0000`
            | `PackageID`           | `REG_SZ`      | `CanonicalGroupLimited.UbuntuonWindows_2004.2020.812.0_x64__79rhkp1fndgsc`
            | `PackageRootFolder`   | `REG_SZ`      | `C:\Program Files\WindowsApps\CanonicalGroupLimited.UbuntuonWindows_2004.2020.812.0_x64__79rhkp1fndgsc`
            | `PackageSid`          | `REG_BINARY`  | ...
            | `SupportedUsers`      | `REG_DWORD`   | 1
