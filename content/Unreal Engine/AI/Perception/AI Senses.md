
# AI Sense Sight
## About
By default you will only get an update `OnPerceptionChange` which means `From "visible" to "not visible" or vice versa.`.

To change it to something else like `OnEveryPerception` ("Continuous update whenever target is perceived") you need to subclass the AI Sense and change the `NotifyType` in the constructor.

> See `EAISenseNotifyType NotifyType` in `UAISense`

> [!Warning] Warning about not using `OnPerceptionChange`
> If not using the default behavior `UAISense::WantsUpdateOnlyOnPerceptionValueChange` will be false, making `OnTargetPerceptionForgotten` delegate never called in `UAIPerceptionComponent::ProcessStimuli` when age expired.
> This is setup in the `FAIStimulus` constructor, see how `bWantsToNotifyOnlyOnValueChange` is initialized.

## Custom sight detection per actor
See `IAISightTargetInterface` with `CanBeSeenFrom`, called from `UAISense_Sight::ComputeVisibility`.

Inside the context struct, `IgnoreActor` is the listener BodyActor (`UAIPerceptionComponent::GetBodyActor`), meaning it should be the AI Owning Controller Pawn.
