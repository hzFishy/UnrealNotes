
## Iteration
you can safely remove entries while iterating a TMap IF you use a iterator
*thanks to Daekesh*
```c++
for (auto It = YourMap.CreateIterator(); It; ++It)
{ 
	It.RemoveCurrent();
}
```

## Add memory
When you add a key and value to a TMap, it gets copied to its own memory, meaning whatever was passed will get destroyed (destructor called).

See [[Other/C++/Move semantics (&&)]]


## GetTypeHash

Example:
```c++
struct PREFABSYSTEMEDITOR_API FPSEditorDrawVisualizationPRCPersistentDataBaseKey  
{
    FString OwnerPath;
    
    FString ComponentPath;
    
    bool operator==(const FPSEditorDrawVisualizationPRCPersistentDataBaseKey& Other) const;
	
	friend uint32 GetTypeHash(const FPSEditorDrawVisualizationPRCPersistentDataBaseKey& Self)
{
	    return HashCombineFast(GetTypeHash(Self.OwnerPath), GetTypeHash(Self.ComponentPath));
    }
};
```
