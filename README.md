# G-Renamer File Fix

> Restore files that a G-Renamer / Grenamer infection left hidden on Windows.

[![Platform](https://img.shields.io/badge/platform-Windows-0078D4?logo=windows&logoColor=white)](#requirements)
[![Shell](https://img.shields.io/badge/script-batch-4D4D4D)](#usage)

Some variants of the G-Renamer malware rename files and mark them with the **hidden** and **system** attributes. Removing the malware stops the behaviour, but the files stay invisible because those attributes remain. This one-line batch script clears them recursively, so the original files reappear.

---

## The problem

```
attrib +s +h photo.exe
```

A file with both `+s` and `+h` set does not show in Explorer, even when **Show hidden files** is enabled (Windows hides protected operating-system files by default). The data is intact — only its visibility flags were changed.

## The fix

```bat
attrib -s -h *.exe /D /S
```

| Flag | Meaning |
| --- | --- |
| `-s` | Clear the **system** attribute |
| `-h` | Clear the **hidden** attribute |
| `/D` | Also process folders |
| `/S` | Recurse into all subfolders |

## Usage

1. **Remove the malware first.** Run a full antivirus scan — if the virus is still active, it will hide the files again.
2. Place `G_Fix_Renamer.bat` in the folder where the files are missing (for example `D:\`).
3. Double-click it. Run as administrator if the files live in a protected location.
4. Open the folder again — the affected `.exe` files are visible.

Verify the attributes were cleared (a healthy file shows no `S` or `H`):

```bat
attrib *.exe /S /D
```

## Notes and limitations

- The script targets **`.exe` files only**, matching the behaviour of the original infection.
- To restore every file type, edit the line to:

  ```bat
  attrib -s -h *.* /D /S
  ```

- The script only changes file attributes. It does not delete, rename, or modify file contents.
- Restoring attributes does **not** disinfect the machine — always remove the malware before running it.
- If files are still missing, check for leftover registry entries or scheduled tasks created by the malware.

## Author

**Mohamed Refaay** — [@mohammed20033](https://github.com/mohammed20033)
