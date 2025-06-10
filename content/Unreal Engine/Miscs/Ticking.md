
## Editor
**To enable ticking in editor**<br>
For actors: Override `ShouldTickIfViewportsOnly` and return true.


## Performance wise
- See the tick section in [Myth-busting "Best Practices" in Unreal Engine](https://dev.epicgames.com/community/learning/tutorials/l3E0/myth-busting-best-practices-in-unreal-engine)
- Check [[Unreal Engine/Performance/_Resources (Performance)#Ticking]]


## Custom ticking
You can use `FTSTicker` to have a a function called over a period of time
Example:
```c++
TickDelegate = FTickerDelegate::CreateUObject(this, &UAssetEditorSubsystem::HandleTicker);  
FTSTicker::GetCoreTicker().AddTicker(TickDelegate, 1.f);
```

the `bool` of `FTickerDelegate` is used to determine if we stop ticking or not