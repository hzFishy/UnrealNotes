# Resources
- https://zomgmoz.tv/unreal/AI-Perception/
- [[AI Perception Types]]
- [[AI Senses]]

# How stimuli works
At the end of `UAIPerceptionSystem::Tick`, the perception system will iterate `ListenerContainer`. If the entry is valid and `bHasStimulusToProcess` (in `FPerceptionListener`) is true we call `FPerceptionListener::ProcessStimuli`, which calls `UAIPerceptionComponent::ProcessStimuli`.

This will iterate `StimuliToProcess` and eventually call the available delegates.
At the end `AAIController::ActorsPerceptionUpdated` is called.

# Miscs

## Disable Pawn auto register as stimulus source
In your `DefaultGame.ini` add the following:
```init
[/Script/AIModule.AISense_Sight]
bAutoRegisterAllPawnsAsSources=false
```

## Forget target actors if stimulus expired
Enable `bForgetStaleActors` in AISystem project settings.
