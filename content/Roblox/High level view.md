
# Resources
- [Roblox and Unreal Engine side by side](https://create.roblox.com/docs/en-us/unreal)
- [[Classes & data types]]

# Conventions

Recommend naming conventions [by Roblox](https://create.roblox.com/docs/en-us/luau/variables#best-practices)
- Use `PascalCase` names for class and enum-like objects.
- Use `PascalCase` names for all Roblox APIs. camelCase APIs are mostly deprecated.
- Use `camelCase` names for local variables, member values, and functions.
- Use `LOUD_SNAKE_CASE` names for local constants (variables with values that you don't expect to change.

# Networking

## Scripts
[Scripts Docs](https://create.roblox.com/docs/en-us/projects/data-model#scripts)

- A [Script](https://create.roblox.com/docs/en-us/reference/engine/classes/Script) object represents a script that can only run on the **server**.
- A [LocalScript](https://create.roblox.com/docs/en-us/reference/engine/classes/LocalScript) object represents a script that can only run on the **client**.
- A [ModuleScript](https://create.roblox.com/docs/en-us/reference/engine/classes/ModuleScript) object represents a reusable script that you can [require()](https://create.roblox.com/docs/en-us/reference/engine/globals/LuaGlobals#require) from **both server and client** scripts.

For scripts to behave properly, you must place them in the correct containers in the data model.

## Physics
[Docs](https://create.roblox.com/docs/en-us/projects/client-server)

**Physics**
Physic simulation is replicate between the server and clients when necessary, for performance the owner/responsible of a assembly can be the server of a client.

# Physics
Roblox uses a rigid body physics engine.
By default, all parts in Roblox are rigid bodies and participate in simulated physics, unless otherwise specified.
An assembly is a rigid body, a assembly can be one or multiple part.
# Architecture

## Data model
[Docs](https://create.roblox.com/docs/en-us/projects/data-model)

Every place is represented by a data model, a hierarchy of objects that describe everything about the place. The data model contains all objects that make up the 3D world, such as parts, terrain, lighting, and other environmental elements. It also contains objects that can control runtime behavior, such as scripts that modify properties, call methods and functions, and respond to events that enable dynamic behavior and interactivity.

