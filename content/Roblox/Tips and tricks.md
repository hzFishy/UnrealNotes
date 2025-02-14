

# Settings
- To disable editor keys in play mode, disable `Respect Studio shortcuts when game has focus` in studio settings.

# Client loading

`WaitForChild` is mandatory inside client scripts because we don't know WHEN the client will receive the container/object/script we want.

> [!quote] [Regarding parent and child replication](https://create.roblox.com/docs/en-us/reference/engine/classes/Model)
 > As with all [Instance](https://create.roblox.com/docs/reference/engine/classes/Instance) types, the fact that a parent [Model](https://create.roblox.com/docs/reference/engine/classes/Model) is replicated to a client does not guarantee that all its children are replicated.
 > You can make a Model atomic using `ModelStreamingMode`
