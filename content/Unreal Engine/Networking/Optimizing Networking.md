# Resources
- [How to implement network managers](https://vorixo.github.io/devtricks/network-managers/) (with implementation).
- For CMC see [[Character Movement Component]]
- [[Dormancy and relevancy|Dormancy and relevancy]]
- [[Replication Graph|Replication Graph]]
- [Avoiding Hitches in Networking](https://dev.epicgames.com/community/learning/knowledge-base/eZyq/unreal-engine-avoiding-hitches-in-networking?source=Rkk)

**Allow async load of unloaded replicated assets**
- `net.AllowAsyncLoading`: Allow async loading of unloaded assets referenced in packets. If false the client will hitch and immediately load the asset, if true the packet will be delayed while the asset is async loaded. net.DelayUnmappedRPCs can be enabled to delay RPCs relying on async loading assets.
- `net.DelayUnmappedRPCs`: If true delay received RPCs with unmapped object references until they are received or loaded, if false RPCs will execute immediately with null parameters. This can be used with net.AllowAsyncLoading to avoid null asset parameters during async loads.
