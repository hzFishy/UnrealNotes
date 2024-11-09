
# Data Assets

Example: [https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/](https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/)

**Filtering PDAs using tags**
- On PDA
	- Override `GetAssetRegistryTags(Context)`
	- Add tag to `Context`
- When searching:
 ```c++
 FAssetRegistryModule& AssetRegistryModule = FModulemanager::LoadModuleChecked<FAssetRegistryModule>("AssetRegistry");
 AssetRegistryModule.Get().GetAssetsByTagValues(Tags, OutArray);
 ```

- filter PDAs using tags [Discord message](https://discord.com/channels/187217643009212416/221799439008923648/1248689350674481263)

# Asset Manager

- [https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/](https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/)
- [https://unrealcommunity.wiki/using-the-asset-manager-qj38astq](https://unrealcommunity.wiki/using-the-asset-manager-qj38astq)