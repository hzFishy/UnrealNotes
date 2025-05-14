
> [!Error] Danger of `LoadSynchronous`
> Example of why `LoadSynchronous` is very bad and shouldn't be used in shipped builds (Thanks to Northstar)
> > [!Quote] Northstar
> > 
> > *"Let's say you're loading a map async. It's a big map and it's taking a while. Then you call LoadSynchronous on a DA or something. Map load gets cancelled, DA blocks game thread to load, Map starts loading all over again but blocking the game thread"*
> 
> In other words: *"LoadSync halts all requests in the queue. They can't resume async so SM handles this by sync loading them"*

# Primary Data Asset

Example on why and how to use: [https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/](https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/)

**Filtering PDAs using tags**
*Thanks to Snaps*
- On PDA
	- Override `GetAssetRegistryTags(Context)`
	- Add tag to `Context`
- When searching:
 ```c++
 FAssetRegistryModule& AssetRegistryModule = FModulemanager::LoadModuleChecked<FAssetRegistryModule>("AssetRegistry");
 AssetRegistryModule.Get().GetAssetsByTagValues(Tags, OutArray);
 ```

- filter PDAs using tags [Discord message](https://discord.com/channels/187217643009212416/221799439008923648/1248689350674481263)

## Asset Bundles
See **Asset Bundles** section [here](https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/)
To include nested asset bundles see [IncludeAssetBundles](https://unreal-garden.com/docs/uproperty/#includeassetbundles)

# Asset Manager

- [https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/](https://www.tomlooman.com/unreal-engine-asset-manager-async-loading/)
- [https://unrealcommunity.wiki/using-the-asset-manager-qj38astq](https://unrealcommunity.wiki/using-the-asset-manager-qj38astq)

