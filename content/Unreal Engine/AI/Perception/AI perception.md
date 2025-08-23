
https://zomgmoz.tv/unreal/AI-Perception/

# AI Sense Sight
By default you will only get a update `OnPerceptionChange` which means `From "visible" to "not visible" or vice versa.`.

To change it to something else like `OnEveryPerception` (`Continuous update whenever target is perceived.`) you need to subclass the AI Sense and change the `NotifyType` in the constructor.

> See `EAISenseNotifyType NotifyType` in `UAISense`

> [!Warning] Warning about not using `OnPerceptionChange`
> if not using that `bWantsToNotifyOnlyOnValueChange` will be false, making `OnTargetPerceptionForgotten` delegate never called when age expired.

# Custom sight detection per actor
See `IAISightTargetInterface` with `CanBeSeenFrom`

