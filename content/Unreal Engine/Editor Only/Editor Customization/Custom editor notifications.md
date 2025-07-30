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
