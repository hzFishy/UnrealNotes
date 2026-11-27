
# Resources
- [Visualizing EVERYTHING in Unreal’s Editor Viewport](https://haukethiessen.com/visualizing-everything-in-unreals-editor-viewport/) (and the [git repo](https://codeberg.org/HaukeThiessen/AdditionalDebugViews/src/branch/main/Source/AdditionalDebugViews/Private/AdditionalDebugViewsSubsystem.cpp))

# Core
To get `ENABLE_ACTOR_PRIMITIVE_COLOR_HANDLER` use `#include "GameFramework/ActorPrimitiveColorHandler.h"`.

You can use `ShowFlag.Wireframe 1` to force wireframe mode, combined with a selected Actor Coloration. Or use `ShowFlag.Lighting 0` to disable any baked lighting for a "unlit" view.

# Examples

```c++
// add

#if ENABLE_ACTOR_PRIMITIVE_COLOR_HANDLER
	FActorPrimitiveColorHandler::Get().RegisterPrimitiveColorHandler(
		NAME_MobilityColor,
		INVTEXT("FU: Mobility"),
		[this] (const UPrimitiveComponent* InPrimitiveComponent) -> FLinearColor 
		{
			FLinearColor OutColor = FLinearColor::Gray;
			
			switch (InPrimitiveComponent->GetMobility())
			{
				case EComponentMobility::Stationary:
					{
						OutColor = FLinearColor::Yellow;
					   break;
					}
				case EComponentMobility::Movable:
					{
						OutColor = FLinearColor::Red;
					   break;
					}
				default:
					break;
			}
			return OutColor + GetColorVariation();
		},
		[]() {},
		INVTEXT("Colorize primitives to show their mobility. Gray = Static, Yellow = Stationary, Red = Movable.")
	);

// remove
FActorPrimitiveColorHandler::Get().UnregisterPrimitiveColorHandler(NAME_MobilityColor);

#endif
```

# Source
## How it works
It calls `UPrimitiveComponent::PushPrimitiveColorToProxy` to the iterated primitives.
Which basically calls `FPrimitiveSceneProxy::SetPrimitiveColor_GameThread` which sets `PrimitiveColor` and requests a GPU update.
