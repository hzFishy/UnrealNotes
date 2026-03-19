
You can apply custom `CanEditChange` rules per `FProperty` using the `UObject::CanEditChange` function.

Example:
```c++
if (InProperty->GetFName() == GET_MEMBER_NAME_CHECKED(ThisClass, SomeVar))  
{  
    return false;  
}
```

If you want to HIDE, there isn't a direct virtual, but Aquanox on Unreal Source Discord shared this little trick:
```c++
void MyModule::StartupModule()
{
#if WITH_EDITOR
	StaticClass<UMyClass>()->FindPropertyByName("SomeVar")->SetMetaData("EditConditionHides", "true");
#endif
}
```

Which works if `EditCondition` is also set:
```c++
auto* SomeProperty = StaticClass<USomeClass>()->FindPropertyByName(GET_MEMBER_NAME_CHECKED(USomeClass, SomePropertyName));  
SomeProperty->SetMetaData("EditCondition", "false");  
SomeProperty->SetMetaData("EditConditionHides", "true");
```
