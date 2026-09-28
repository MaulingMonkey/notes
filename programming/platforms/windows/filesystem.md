## Notes
-   [Windows Projected File System (ProjFS)](https://learn.microsoft.com/en-us/windows/win32/projfs/projected-file-system)
-   [What's the deal with those reserved filenames like NUL and CON?](https://devblogs.microsoft.com/oldnewthing/20031022-00/?p=42073) (The Old New Thing)
-   <https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file>
-   <https://ss64.com/nt/syntax-filenames.html>

## Quirks
-   8.3 Names
-   `Progra~1` → `Program Files`
-   `CON` and other reserved filenames
-   Case Sensitivity
-   FAT32 lacks filesystem security vs NTFS
-   [`\\?\...`](https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file#win32-file-namespaces) &mdash; Win32 File Namespace
-   [`\\.\...`](https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file#win32-device-namespaces) &mdash; device namespace
-   [NT Namespace](https://learn.microsoft.com/en-us/windows/win32/fileio/naming-a-file#nt-namespaces)
