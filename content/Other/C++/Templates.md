
# Make type in template with defined base
```c++
template<std::derived_from<UPrimitiveComponent> PrimitiveClass>  
struct FPTAPushedEntryBase<UPrimitiveComponent>
```

now if you do something like `struct FPTAPushedEntryStaticMesh : public FPTAPushedEntryBase<USceneComponent>` you will get a compiler error, because `USceneComponent` is a parent of `UPrimitiveComponent`.


