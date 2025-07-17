
# Resources
- [UE Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/smart-pointers-in-unreal-engine)

## TSharedPtr
If you call `.Reset()` this will call the destructor of the held object if no other references exists.
