

# Settings
- To disable editor keys in play mode, disable `Respect Studio shortcuts when game has focus` in studio settings.

# Client loading

`WaitForChild` is mandatory inside client scripts because we don't know WHEN the client will receive the container/object/script we want.
But if you call `WaitForChild` for a asset, it isn't necessary to call `WaitForChild` for childs.
