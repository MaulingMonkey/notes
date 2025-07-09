## Notes
*   Key names: Any character except:
    *   NUL, which terminates the key name
    *   Backslash, which is treated as a seperator between keys and subkeys
*   Value name: Any character except:
    *   NUL, which terminates the value name
    *   (Backslashes are OK!)
*   Value values can contain whatever (including NULs, although that's sketchy for string values?)
*   16-bit Windows did not have value names or types.  Each key only had a single value, which was always a string, and can still be accessed on modern 32-bit Windows via a null value name (displayed as e.g. `(Default)` in regedit etc.)

## `*.reg` File Format

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate]
"DisableOSUpgrade"=dword:00000001
```

TODO:
* See how value names are escaped, encoding of high unicode values
* See how key names with `]` are encoded (maybe not escaped at all, and maybe that's okay if you just assume the line starts and ends with `[` and `]`?)

## References
*   [Structure of the Registry](https://learn.microsoft.com/en-us/windows/win32/sysinfo/structure-of-the-registry) (learn.microsoft.com)
*   [What is the Windows `(Default)` Registry Key?](https://superuser.com/a/1715543)
*   [Why do registry keys have a default value?](https://devblogs.microsoft.com/oldnewthing/20080118-00/?p=23773) (The Old New Thing)
