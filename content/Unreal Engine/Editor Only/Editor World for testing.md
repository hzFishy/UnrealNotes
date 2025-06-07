## How it's done elsewhere
- [`FTestWorld` from ue5coro](https://github.com/landelare/ue5coro/blob/bf83ab3ac7195d8ea6f2dd7dce0d5f2f92e48bc1/Source/UE5CoroTests/Private/TestWorld.cpp#L37)
- `FActorTestSpawner` in engine CQTest module]

# My implementation
*Thanks to Northstar and Aquanox (Unreal Source Discord) for the help, snippets and directions.*

By just doing `TestWorldContainer->Init()` you are making a new GI and World. This allows you to do a lot of things, such as automated tests and spawning actors.

*Some people likes to do this in constructor, I don't because sometimes i'm declaring the test container but i don't yet want to create a test world, etc

**Code to make this test GI and World (with SpawnActor and destruction)**
[Gist link](https://gist.github.com/hzFishy/bea474a2f684ac8bac107cce4d1e11cd)

