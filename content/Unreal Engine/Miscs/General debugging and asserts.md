## Asserts
- `check`
	- Failing a `check` will bring up the debugger when using an IDE and cause a UE crash if not.
	- Doesn't return the boolean.
- `verify`
	- Failing a `verify` will bring up the debugger when using an IDE and not cause a UE crash if not.
	- Doesn't return the boolean.
- `ensure`
	- Failing a `ensure` will bring up the debugger when using an IDE and not cause a UE crash if not. It can print a custom text in the logs.
	- Returns the boolean.

## Engine notifications
Use `FNotificationInfo` with `FSlateNotificationManager::Get().AddNotification(Info);`
Then use `SetCompletionState` to set the status (success, fail, ...).

Snippet example (thanks to Hojo, Unreal Source Discord)
```c++
void PostNotificationInfo_Warning(FText Title, FText Description, float Duration)
{
	FNotificationInfo NotificationInfo(Title);
	NotificationInfo.ExpireDuration = Duration;
	NotificationInfo.Image = FAppStyle::GetBrush("Icons.WarningWithColor");
	NotificationInfo.SubText = Description;
	FSlateNotificationManager::Get().AddNotification(NotificationInfo);
}
```

To use `FAppStyle` you need to include `SlateCore` module.

You can find another simple example at `UMassEntityConfigAsset::ValidateEntityConfig`