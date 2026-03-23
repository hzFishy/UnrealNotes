
This can be registered to the `FSlateApplication` anytime.
Snippet:
```c++
PlayerInputProcessor = MakeShared<FPlayerInputProcessor>();
FSlateApplication::Get().RegisterInputPreProcessor(PlayerInputProcessor);
```

It will be used all the time, even in editor, so watch out depending on where you register it.
Subclass `IInputProcessor`, the only mandatory function to overwrite is `Tick`.

The input processor is used in the various related functions in the slate application class.

For example `FSlateApplication::ProcessMouseButtonUpEvent` will call `HandleMouseButtonUpEvent` on all registered processors.
The first processor that returns `true` will break the loop and override the default slate app behavior for that event.
If its not overridden it will go to `FSlateApplication::RoutePointerUpEvent`, which in this example will call `SViewport::OnMouseButtonUp` and `FSceneViewport::OnMouseButtonUp`.

