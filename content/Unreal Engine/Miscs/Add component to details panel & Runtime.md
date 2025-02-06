**Snippet to add a component at runtime and see it in detail panel**
<br>Thanks Northstar for sharing.
```c++
UActorComponent* UCyComponentLibrary::AddRuntimeInstanceComponent(AActor* Actor, TSubclassOf<UActorComponent> ComponentClass) 
{
	if (!IsValid(Actor) || !IsValid(ComponentClass)) { return nullptr; }
	
	EObjectFlags Flags = RF_Transient;
	Flags &= ~RF_Transactional;
	
	UActorComponent* NewComponent = NewObject<UActorComponent>(Actor, ComponentClass, NAME_None, Flags);
	if (!IsValid(NewComponent)) { return nullptr; }
	
	NewComponent->RegisterComponent();
	Actor->AddInstanceComponent(NewComponent);
	
	return NewComponent;
}
```


**Never tested**
<br>Can be needed when dynamically spawning components
```c++
FKismetEditorUtilities::AddComponentsToBlueprint
```

**For scene components**
when you attach them and register, there is only two ways
1. Use SetupAttachement then RegisterComponent
2. Use RegisterComponent then AttachTo

