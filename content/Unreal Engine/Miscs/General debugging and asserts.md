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

