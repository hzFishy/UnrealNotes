

> [!Error] Iris notice
> See [[Iris Fast Array]]

# Resources
- See `FastArraySerializer.h` (Explanation of why and how + code examples)
- See [this article](https://ikrima.dev/ue4guide/networking/network-replication/fast-tarray-replication/)

# Declaration

In your class, you must add `UPROPERTY(Replicated)` above your container, and add it like usual in `GetLifetimeReplicatedProps`.

```c++
USTRUCT()
struct FExampleItemEntry : public FFastArraySerializerItem
{
	GENERATED_BODY()
	
	UPROPERTY()
	int32 ExampleIntProperty;	
	
	void PreReplicatedRemove(const struct FExampleArray& InArraySerializer);
	void PostReplicatedAdd(const struct FExampleArray& InArraySerializer);
	void PostReplicatedChange(const struct FExampleArray& InArraySerializer);
	
	// Optional: debug string used with LogNetFastTArray logging
	FString GetDebugString();
	
};

USTRUCT()
struct FExampleArray: public FFastArraySerializer
{
	GENERATED_BODY()

	UPROPERTY()
	TArray<FExampleItemEntry>	Items;


	bool NetDeltaSerialize(FNetDeltaSerializeInfo& DeltaParms)
	{
	   return FFastArraySerializer::FastArrayDeltaSerialize<FExampleItemEntry, FExampleArray>( Items, DeltaParms, *this );
	}
};

template<>
struct TStructOpsTypeTraits< FExampleArray > : public TStructOpsTypeTraitsBase2< FExampleArray >
{
       enum 
       {
			WithNetDeltaSerializer = true,
       };
};
```

# Replication
See [[Replication]] and `FRepLayout::DeltaSerializeFastArrayProperty`.
