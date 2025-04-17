
- [Actor Load/Init Function Cheatsheet](https://ikrima.dev/ue4guide/gameplay-programming/actor-tick-lifecycle-flow/actor-lifecycle-diagram/)

- `PostTransacted` is called when you move/edit anything in a BP
- When you compile a BP, `PostInitProperties`, `PostEditChangeProperty` and `PostCDOContruct` are called.
- When you edit a property `PostEditChangeProperty` is called (Doesn't work for Transforms)
- `PostCDOContruct` always has its World null.