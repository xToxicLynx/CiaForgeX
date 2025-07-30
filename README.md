# CiaForgeX

**A Unity editor tool to export 3DS builds as CIA files – with system mode overrides and extended memory support**

Modifies and rebuilds with `.rsf` injection

---

## Features

* Supports **two export modes**:

  * `Build CIA` – fast export via `3dsconv.exe`
  * `Extract, Modify and Rebuild CIA` – for advanced memory configs & flag injection
* Extended memory options:

  * Old 3DS: `32MB`, `72MB`, `80MB`, `96MB`
  * New 3DS: `124MB`, `178MB`
  * ⚠**Note:** `96MB` Does not currently work (probably never will), as it crashes the game on boot
* Uses standard 3DS dev tools:

  * `ctrtool`, `makerom`, `3dstool`, `rsfgen`, `3dsconv`
* Optional `.xml` cleanup after export
* Automatically patches RSF
* Exports CIA to same folder as `.cci` and opens it in Explorer when finished

## Usage

### Build CIA (Quick Mode)

* Simply exports your `.cci` as a `.cia` using `3dsconv.exe`
* Best if you're not using extended memory or modifying the RSF

### Extract, Modify and Rebuild CIA

* Fully rebuilds a CIA using extracted parts from `.cci`
* Applies `.rsf` modifications (SystemMode, N3DS RAM, debug flags)

---

## RSF Injection Notes

The tool generates a new `game.rsf` using `rsfgen`, and injects key configuration lines, such as:

**EXAMPLE**
```rsf
SystemMode                   : 80MB # This is the SystemMode setting for Old 3ds
SystemModeExt                : 178MB # This is the SystemMode setting for New 3ds
CanWriteSharedPage           : true
CanUseNonAlphabetAndNumber   : true
SpecialMemoryArrange         : true
PermitMainFunctionArgument   : true
```

---

## Clean Build

After export, temp files are cleared and your resulting `.cia` is selected in Explorer

---

## Tools Used

* [3dstool](https://github.com/dnasdw/3dstool) - Extracts and rebuilds .cci and .cia components
* [ctrtool](https://github.com/3DSGuy/Project_CTR) - Reads and extracts data from 3DS files
* [makerom](https://github.com/profi200/Project_CTR) - Rebuilds CIA files from extracted contents using a patched RSF file
* [3dsbatchdecryptor](https://gbatemp.net/threads/batch-cia-3ds-decryptor-a-simple-batch-file-to-decrypt-cia-3ds.512385/) - Batch script used to decrypt original CIA/3DS files before rebuilding
* [rsfgenpy3](https://github.com/CooingMaxito/rsfgen-python3) - Based on rsfgen by ihaveamac, updated to Python 3
