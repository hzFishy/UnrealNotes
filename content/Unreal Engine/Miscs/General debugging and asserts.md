## Asserts
- Failing a `check` will bring up the debugger when using an IDE and cause a UE crash if not.
- Failing a `verify` will bring up the debugger when using an IDE and not cause a UE crash if not.
- Failing a `ensure` will not bring up the debugger when using an IDE and not cause a UE crash if not. But it can print a custom text in the logs.

## Engine notifications
Use `FNotificationInfo` with `FSlateNotificationManager::Get().AddNotification(Info);`

