
# Resources
- [Visualizing EVERYTHING in Unreal’s Editor Viewport](https://haukethiessen.com/visualizing-everything-in-unreals-editor-viewport/) (and the [git repo](https://codeberg.org/HaukeThiessen/AdditionalDebugViews/src/branch/main/Source/AdditionalDebugViews/Private/AdditionalDebugViewsSubsystem.cpp))

# Examples

```c++
FActorPrimitiveColorHandler::Get().RegisterPrimitiveColorHandler(
		NAME_MobilityColor,
		LOCTEXT("MobilityColor", "Mobility"),
		[](const UPrimitiveComponent* InPrimitiveComponent) -> FLinearColor {
			const float Variation = FMath::FRand() * 0.2 - 0.1;
			FLinearColor OutColor = FLinearColor::Gray;
			switch (InPrimitiveComponent->GetMobility())
			{
			case EComponentMobility::Stationary:
				OutColor = FLinearColor::Yellow;
				break;
			case EComponentMobility::Movable:
				OutColor = FLinearColor::Red;
				break;
			}
			return OutColor + FLinearColor(Variation, Variation, Variation);
		},
		[]() {},
		LOCTEXT(
			"MobilityColor_ToolTip",
			"Colorize primitives to show their mobility. Gray = Static, Yellow = Stationary, Red = Movable."));
```
