# CiaForgeX

**A Unity editor tool to export 3DS builds as CIA files – with eXtended memory support** (system mode overrides)

- Modifies and rebuilds with `.rsf` injection
- Hooks up to the Unity/CTR-SDK build chain, executes automatically after each build

Many thanks to [@CooingMaxito](https://www.github.com/CooingMaxito), who wrote the majority of this README and also contributed to the tool itself

---

## 📖Table of contents

[1. Features✨](#features)<br>
[2. Usage⚙️](#usage)<br>
[3. RSF Injection Notes💉](#rsf-injection-notes)<br>
[4. Troubleshooting🚩](#troubleshooting)<br>
[5. Tools Used🔧](#tools-used)<br>
<br>

---

## ✨Features

* Supports **two export modes**:

  * `Build CIA` – fast export via `3dsconv.exe`
  * `Extract, Modify and Rebuild CIA` – for advanced memory configs & flag injection
* **Extended memory** options:

  * Old 3DS: `32MB`, `72MB`, `80MB`, `96MB`
  * New 3DS: `124MB`, `178MB`
  * ⚠️**Note:** `96MB` Does not currently work (probably never will), as it crashes the game on boot
* Uses standard **3DS dev tools**:

  * `ctrtool`, `makerom`, `3dstool`, `rsfgen`, `3dsconv`
* Automatically **patches RSF** file
* **Temp** file **cleanup** + Optional `.cci` and `.xml` **cleanup** after export
* Exports `.cia` to same folder as `.cci` and opens it in Explorer when finished
<br>

---

## ⚙️Usage
After importing the .unitypackage, select `3DS` > `CiaForgeX` and the configuration window will appear
> Select one of the following modes:

#### Build CIA (Quick Mode)

* Simply exports your `.cci` as a `.cia` using `3dsconv.exe`
* Best if you're not using extended memory or modifying the RSF
* If you're just looking for a way to automatically export your games to `.cia`  format, use this mode

#### Extract, Modify and Rebuild CIA

* Fully rebuilds a CIA using extracted parts from `.cci`
* Applies `.rsf` modifications (SystemMode, N3DS RAM, debug flags)
* **Breakdown:**
	* [Extracts the CCI's content](https://github.com/ihaveamac/3DS-rom-tools/wiki/Extract-a-game-or-application-in-.3ds-or-.cci-format#method-1-decrypted-or-zero-key-encrypted-cci-with-ctrtool)
	* [Generates an rsf file](https://github.com/CooingMaxito/rsfgen-python3?tab=readme-ov-file#usage) and modifies it
	* [Rebuilds the rom as CIA](https://github.com/ihaveamac/3DS-rom-tools/wiki/Rebuilding-a-game-or-application-in-.cia-format-using-RSF-files#rebuilding-based-on-a-cci-3dscci-using-rsf)
	

> Configure the remaining settings according to your needs and the tool is ready for use

<br>

---

## 💉RSF Injection Notes

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
For more information on rsf files check [here](https://github.com/3DSGuy/Project_CTR/blob/master/makerom/README.md#creating-rsf-files) and this [sample rsf](https://gist.github.com/jakcron/9f9f02ffd94d98a72632)
<br>
<br>

---

## 🚩Troubleshooting

not today
<br>

---

## 🔧Tools Used

* [3dstool](https://github.com/dnasdw/3dstool) - Extracts and rebuilds .cci and .cia components [@dnasdw](https://github.com/dnasdw)
* [ctrtool](https://github.com/3DSGuy/Project_CTR) - Reads and extracts data from 3DS files [@3DSGuy](https://github.com/3DSGuy)
* [makerom](https://github.com/3DSGuy/Project_CTR) - Rebuilds CIA files from extracted contents using a patched RSF file [@3DSGuy](https://github.com/3DSGuy)
* [3dsbatchdecryptor](https://gbatemp.net/threads/batch-cia-3ds-decryptor-a-simple-batch-file-to-decrypt-cia-3ds.512385/) - Batch script used to decrypt original CIA/3DS files before rebuilding [@matiffeder](https://github.com/matiffeder)
* [rsfgenpy3](https://github.com/CooingMaxito/rsfgen-python3) - Based on rsfgen by ihaveamac, updated to Python 3 [@CooingMaxito](https://github.com/CooingMaxito)
