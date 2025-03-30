
## In short
A `UPackage` is the container for anything stored on disk. It holds one or more `UObject`s and functions as the outermost container object for any `.uasset`.


## In depth

> [!Quote] What **was** and **is now** a `UPackage` from Nick Darnell (from BenUI's Discord server)
> *Thanks to Eren (Unreal Source Discord) for sharing the message*
> ![[Pasted image 20250330165923.png]]
> 
> **In Text:**
> *"A long time ago, in UE1,2,3, things like textures, levels, meshes...etc we're stored on a package, you might place all of your games textures into a single texture package.*
> 
> *This of course, is madness. It meant that if you were an artist on the team and wanted to import a new texture you had to checkout and checkin a package that might be several hundred megs. Lame.*
> 
> *In early pre-UE4, they decided to move to one asset per package. So just like before the outer of all assets, textures, blueprints, whatever is always a UPackage. It's the container for anything stored on disk. It's also used for temporary storage like, the outermost of any arbitrarily constructed UObject that doesn't have a specific outer gets the transient package.*
> 
> *So that's all it is, a package holds one or more UObjects and functions as the outermost container object for any uasset"*

*Thanks Ollsonn Northsar and Baffled for the following*

Also, all reflected c++ types are stored in the packages `/Script/ModuleName`.
`ModuleName` here is a "package", `/Script/ModuleName` is the path to the module "package" on the virtual filesystem. The `ModuleName` isn't a real package, the module in that path is saying where that package belongs.

**Note:** `Script/` is for c++ files and `Game/` is for assets.

