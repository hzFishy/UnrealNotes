# Resources
- Great tool to have fast bindings: [MDFastBinding](https://github.com/DoubleDeez/MDFastBinding)


# About
Function binding is created in `SPropertyBinding::HandleCreateAndAddBinding`.
It runs EACH tick, so unless it is something that is expected to change each frame I wouldn't recommend it and instead use a event based approached.

You can prevent binding creation with this setting: <br>
![[Pasted image 20251013141250.png]]