# AssetStudio-Arknights

## Fork Info

This fork is tested only on Arknights, and problems about other apps will be ignored.

## Changes

* New feature: Export structured.
* New featrue: Export to JSON (Dump, Asset List).
* New featrue: UI Language Setting.
* Enhancement: Improved preview of mesh, audio.

## Arknights Compatibility Fixes (HX0922 Fork)

This fork includes fixes for Arknights asset bundle quirks:

* **Bundle alignment fix**: Arknights tampers with the raw `version` field in UnityFS bundles. Use parsed Unity revision string (e.g. `"2021.3.39f1"`) instead to determine 16-byte block alignment. Fixes loading of `[pack]common.ab` and similar shared bundles. (Ref: Perfare/AssetStudio#869)

* **LZ4AK decompression**: Arknights v2.5.04+ uses a custom LZHAM variant (LZ4AK) that byte-swaps nibbles/offsets before LZ4 decompression. Handled in `BundleFile.ReadBlocks()`.

* **Shader compatibility**: Arknights' Shader binary format may be incompatible with AssetStudio's parser. Silently falls back to generic Object instead of logging errors.

## Prebuilt Release

See [Releases](https://github.com/HX0922/AssetStudio-Arknights/releases) for prebuilt Windows x64 binaries.

[日志](./doc/changes.zh.md)

# Original Readme Below

**None of the repo, the tool, nor the repo owner is affiliated with, or sponsored or authorized by, Unity Technologies or its affiliates.**

AssetStudio is a tool for exploring, extracting and exporting assets and assetbundles.

## Features
* Support version:
  * 3.4 - 2022.1
* Support asset types:
  * **Texture2D** : convert to png, tga, jpeg, bmp
  * **Sprite** : crop Texture2D to png, tga, jpeg, bmp
  * **AudioClip** : mp3, ogg, wav, m4a, fsb. support convert FSB file to WAV(PCM)
  * **Font** : ttf, otf
  * **Mesh** : obj
  * **TextAsset**
  * **Shader**
  * **MovieTexture**
  * **VideoClip**
  * **MonoBehaviour** : json
  * **Animator** : export to FBX file with bound AnimationClip

## Requirements (Prebuilt Binary)

The prebuilt release in [Releases](https://github.com/HX0922/AssetStudio-Arknights/releases) is a **framework-dependent** Windows x64 build.

- [.NET Desktop Runtime 8.0 (x64)](https://dotnet.microsoft.com/download/dotnet/8.0) — download the **Windows x64** installer under ".NET Desktop Runtime"
- Windows 10/11 x64

## Build

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- Windows: Visual Studio 2022+ with C++ toolchain (for `AssetStudioFBXNative` and `Texture2DDecoder`)
- Linux cross-compile: add `-p:EnableWindowsTargeting=true`, use prebuilt native DLLs from a prior Windows build for `AssetStudioFBXNative` and `Texture2DDecoder`

```
dotnet publish AssetStudioGUI/AssetStudioGUI.csproj -c Release -r win-x64 -p:EnableWindowsTargeting=true
```

- **AssetStudioFBXNative** requires [FBX SDK 2020.2.1](https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2020-2-1) when building on Windows


## Usage

### Load Assets/AssetBundles

Use **File-Load file** or **File-Load folder**.

When AssetStudio loads AssetBundles, it decompresses and reads it directly in memory, which may cause a large amount of memory to be used. You can use **File-Extract file** or **File-Extract folder** to extract AssetBundles to another folder, and then read.

### Extract/Decompress AssetBundles

Use **File-Extract file** or **File-Extract folder**.

### Export Assets

use **Export** menu.

### Export Model

Export model from "Scene Hierarchy" using the **Model** menu.

Export Animator from "Asset List" using the **Export** menu.

#### With AnimationClip

Select model from "Scene Hierarchy" then select the AnimationClip from "Asset List", using **Model-Export selected objects with AnimationClip** to export.

Export Animator will export bound AnimationClip or use **Ctrl** to select Animator and AnimationClip from "Asset List", using **Export-Export Animator with selected AnimationClip** to export.

### Export MonoBehaviour

When you select an asset of the MonoBehaviour type for the first time, AssetStudio will ask you the directory where the assembly is located, please select the directory where the assembly is located, such as the `Managed` folder.

#### For Il2Cpp

First, use my another program [Il2CppDumper](https://github.com/Perfare/Il2CppDumper) to generate dummy dll, then when using AssetStudio to select the assembly directory, select the dummy dll folder.

## Open source libraries used

### Texture2DDecoder
* [Ishotihadus/mikunyan](https://github.com/Ishotihadus/mikunyan)
* [BinomialLLC/crunch](https://github.com/BinomialLLC/crunch)
* [Unity-Technologies/crunch](https://github.com/Unity-Technologies/crunch/tree/unity)
