
# Debug package
You can use `UnrealPak.exe` at `Engine/Binaries/Win64` to debug/extract the `.pak`, `.utoc`, `.ucas` file(s).

**Commands [Docs](http://dev.epicgames.com/documentation/unreal-engine/zen-loader-in-unreal-engine#inspectingthecontentsofcontainerfiles)**
- `UnrealPak.exe MyFile.pak -list`
- `-diff`
- `UnrealPak.exe MyFile.pak/.utoc -extract MyDestination`

# About `.pak`, `.ucas` & `.utoc`

Since UE5 [Zen Loader](https://dev.epicgames.com/documentation/unreal-engine/zen-loader-in-unreal-engine) the files you find in the `Paks` folder in your packaged build are different.

- `.ucas`: Content Addressable Store, it holds all of your cooked assets.
- `.utoc`: Table Of Contents that is used by the engine to quickly find an asset in the `.ucas` file.

The global file is an offline computed dependency graph for your assets.
The upside to using the io store is a noticeable improvement to loading times.

