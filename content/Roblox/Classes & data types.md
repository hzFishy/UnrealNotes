- [All classes](https://create.roblox.com/docs/en-us/reference/engine/classes/)

# Services

[Services Docs](https://create.roblox.com/docs/en-us/scripting/services) & [Object organization docs](https://create.roblox.com/docs/en-us/projects/data-model#object-organization)
## Workspace
Handles objects that exist in the 3D world, such as `BasePart`s and `Attachment`s.

## Environment
Containers like [Lighting](https://create.roblox.com/docs/en-us/reference/engine/classes/Lighting) and [SoundService](https://create.roblox.com/docs/en-us/reference/engine/classes/SoundService) that contain objects for environmental settings and elements.

## Replication
Containers for content and logic that replicates between the server and client, such as [ReplicatedStorage](https://create.roblox.com/docs/en-us/reference/engine/classes/ReplicatedStorage) and [ReplicatedFirst](https://create.roblox.com/docs/en-us/reference/engine/classes/ReplicatedFirst).

## Server
Containers for server-side only content and logic, such as [ServerScriptService](https://create.roblox.com/docs/en-us/reference/engine/classes/ServerScriptService) and [ServerStorage](https://create.roblox.com/docs/en-us/reference/engine/classes/ServerStorage).

## Client
Containers for client-side content and logic, such as [StarterPlayer](https://create.roblox.com/docs/en-us/reference/engine/classes/StarterPlayer) and [StarterGui](https://create.roblox.com/docs/en-us/reference/engine/classes/StarterGui).

## Chat
Containers for objects that enable chat features, such as [VoiceChatService](https://create.roblox.com/docs/en-us/reference/engine/classes/VoiceChatService) and [TextChatService](https://create.roblox.com/docs/en-us/reference/engine/classes/TextChatService).

# Basics

## Core

[Instance](https://create.roblox.com/docs/en-us/reference/engine/classes/Instance) is the base class for all classes in the Roblox class hierarchy which can be part of the Data Model tree.

[LuaSourceContainer](https://create.roblox.com/docs/en-us/reference/engine/classes/LuaSourceContainer) is the base class for all objects which container Lua code.

[Camera](https://create.roblox.com/docs/en-us/reference/engine/classes/Camera) is the player camera. See [here](https://create.roblox.com/docs/en-us/workspace/camera) for examples on how to edit the default camera behaviors.

## Parts classes
- [BasePart](https://create.roblox.com/docs/en-us/reference/engine/classes/BasePart) is an abstract base class for in-world objects. ([Docs](https://create.roblox.com/docs/en-us/reference/engine/classes/BasePart))

- A [Part](https://create.roblox.com/docs/en-us/reference/engine/classes/BasePart) is a subclass of `BasePart`, it can be one of the five primitive shape.

- A [MeshPart](https://create.roblox.com/docs/en-us/reference/engine/classes/MeshPart) is a subclass of `BasePart`, used for custom meshes. There is also [SpecialMesh](https://create.roblox.com/docs/en-us/reference/engine/classes/SpecialMesh)and other specific mesh types.


## Containers
- A [Model](https://create.roblox.com/docs/en-us/reference/engine/classes/Model) is a container for objects, they are intended to represent geometric groupings.
- A [Folder](https://create.roblox.com/docs/en-us/reference/engine/classes/Folder) is a container for scripts.


## Scripts

**Script** ([Docs](https://create.roblox.com/docs/en-us/reference/engine/classes/Script))
Can access server-side objects, properties and events.

**LocalScript** ([Docs](https://create.roblox.com/docs/en-us/reference/engine/classes/LocalScript))
Runs on the client side. It is used to access client-only objects.

**ModuleScript** ([Docs](https://create.roblox.com/docs/en-us/reference/engine/classes/ModuleScript))
Used with `require`, returns only one value.


# Data types

A `CFrame` holds a location and a rotation
