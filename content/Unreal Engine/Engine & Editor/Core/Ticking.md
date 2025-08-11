
## Editor
**To enable ticking in editor**<br>
For actors: Override `ShouldTickIfViewportsOnly` and return true.


## Performance wise
- Use `dumpticks` will give you all enabled and disabled tick info for level
- See the tick section in [Myth-busting "Best Practices" in Unreal Engine](https://dev.epicgames.com/community/learning/tutorials/l3E0/myth-busting-best-practices-in-unreal-engine#don'tusetick?)
- [Aggregating Ticks to Manage Scale in Sea of Thieves | Unreal Fest Europe 2019 | Unreal Engine](https://www.youtube.com/watch?v=CBP5bpwkO54)


## Custom ticking
You can use `FTSTicker` to have a a function called over a period of time
Example:
```c++
TickDelegate = FTickerDelegate::CreateUObject(this, &UAssetEditorSubsystem::HandleTicker);  
FTSTicker::GetCoreTicker().AddTicker(TickDelegate, 1.f);
```

the `bool` of `FTickerDelegate` is used to determine if we stop ticking or not