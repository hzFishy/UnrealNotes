
The billboard component is stored as the `SpriteComponent`.

# Access
To get this component, you either subclass your root scene component, or from the owning actor you get the component by class using `UBillboardComponent`.

> [!Warning] A single actor can hold multiple billboard components.

# Creation
When `UActorComponent::ExecuteRegisterEvents` is ran, `USceneComponent::CreateSpriteComponent` will be called.
If `bVisualizeComponent`is true, the billboard component will be created, the sprite loaded and set and so on...

It seems that billboard components are usually created with an null sprite texture, so it uses the default sprite texture.
Some component light the light components uses `SetSprite` on the billboard component inside `OnRegister` (since the billboard component will be created at this point).
