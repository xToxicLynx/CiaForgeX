# CiaForgeX

**A Unity editor tool to export 3DS builds as CIA files – with eXtended memory support** (system mode overrides)

- Modifies and rebuilds with `.rsf` injection
- Hooks up to the Unity/CTR-SDK build chain, executes automatically after each build

Many thanks to [@CooingMaxito](https://www.github.com/CooingMaxito), who wrote the majority of this README and also contributed to the tool itself

---

## 📖Table of contents

[1. Features✨](#features)<br>
[2. Usage⚙️](#%EF%B8%8Fusage)<br>
[3. RSF Injection Notes💉](#rsf-injection-notes)<br>
[4. Troubleshooting🚩](#troubleshooting)<br>
[5. Tools Used🔧](#tools-used)<br>
<br>

---

## ✨Features

* Supports **two export modes**:

  * `Build CIA` – Converts built `.cci` to `.cia` via makerom
  * `Extract, Modify and Rebuild CIA` – for advanced memory configs & flag injection
* **Extended memory** options:

  * Old 3DS: `32MB`, `72MB`, `80MB`, `96MB`
  * New 3DS: `124MB`, `178MB`
  * ⚠️**Note:** `96MB` Does not currently work (probably never will), as it crashes the game on boot
* **N3DS specific** options:

  * Cpu Speed: `268MHz`, `804MHz`
  * Enable L2 Cache: `true`/`false`
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

* Simply converts your `.cci` as a `.cia` using `makerom.exe`
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

In case you experience crashes trying to load up your game using a different SystemMode **other than** the **default (64 MB)**, try following these steps:

1. Create a new (empty) project
2. Switch the platform to `Nintendo 3DS` in build settings
3. Configure `PlayerSettings` (exclusively `Publishing Settings`) like this:
<br>

![](https://i.imgur.com/4L22CPa.png)
<br>

4. Save the current scene, add it to the scenes list in `Build Settings`
5. Import the [.unitypackage](https://github.com/xToxicLynx/CiaForgeX/releases), select a SystemMode other than `64MB (Default)`
6. Build

Now try running the build on **citra** or your **3ds**. If it doesn't crash (on citra a crash means **Speed 0%**), continue with the next steps:

7.  Right click anywhere in the `Project` window, select `Show in Explorer`
8. Open the ProjectSettings folder, copy `ProjectSettings.asset`
9. Open the ProjectSettings folder of your original project
10. Delete the `ProjectSettings.asset` in it and paste the one you just copied

Open your **original project** (the one in which you had issues building) and try **building** now.

If it runs, configure the rest of `ProjectSettings.asset` to your needs.

<br>

If your **game crashes** again after configuring the PlayerSettings some more, you've probably changed something you shouldn't have.
<br>
For me changing the `Memory` section causes the game to crash (only **with extended memory mode** enabled though, so not 64MB). It also didn't help to revert my changes, it still crashed. That's why I had to create a new empty project and do the `PlayerSettings.asset` procedure all over again.

![](https://i.imgur.com/5bWCskJ.png)

I would recommend making a **backup of** `PlayerSettings.asset` once you get a **working version** of it or make **changes**, just in case you mess something up.

<br>

---

## 🔧Tools Used

* [3dstool](https://github.com/dnasdw/3dstool) - Extracts and rebuilds .cci and .cia components [@dnasdw](https://github.com/dnasdw)
* [ctrtool](https://github.com/3DSGuy/Project_CTR) - Reads and extracts data from 3DS files [@3DSGuy](https://github.com/3DSGuy)
* [makerom](https://github.com/3DSGuy/Project_CTR) - Rebuilds CIA files from extracted contents using a patched RSF file [@3DSGuy](https://github.com/3DSGuy)
* [3dsbatchdecryptor](https://gbatemp.net/threads/batch-cia-3ds-decryptor-a-simple-batch-file-to-decrypt-cia-3ds.512385/) - Batch script used to decrypt original CIA/3DS files before rebuilding [@matiffeder](https://github.com/matiffeder)
* [rsfgenpy3](https://github.com/CooingMaxito/rsfgen-python3) - Based on rsfgen by ihaveamac, updated to Python 3 [@CooingMaxito](https://github.com/CooingMaxito)
