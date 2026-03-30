# Resources
- Check [this detailed medium blog post](https://medium.com/@zima3x/ue5-intermediate-integrating-commonui-and-enhanced-input-into-gameplay-7b35c5502cb4)
- [x157 dev note](https://x157.github.io/UE5/CommonUI/Annotations/EpicGames-Introduction-to-CommonUI.html)

# Common Input Subsystem
To know what current input mode is active, see `OnInputMethodChangedNative`, `OnInputMethodChanged` and `GetCurrentInputType`.

# `UCommonUIActionRouterBase`
A subsystem that can be overridden by simply creating a subclass.

# Miscs

## Left Mouse Button triggered when pressing Gamepad bottom button.

This happens because of `FCommonAnalogCursor::ShouldVirtualAcceptSimulateMouseButton` returning true, which simulate a left mouse button click.
One of the issues with this (in 5.6 as far as I know) is that it will trigger the left mouse button, but it will NEVER stop it, making it triggered forever. To make it stop you have to focus something else to break the loop. This doesn't seem like the wanted behavior because `ShouldVirtualAcceptSimulateMouseButton` is both used in the `FCommonAnalogCursor::HandleKeyDownEvent` and `FCommonAnalogCursor::HandleKeyUpEvent` functions, meaning the cursor should be released.

`FCommonAnalogCursor::ShouldVirtualAcceptSimulateMouseButton` can be overridden or you can control its default behavior with the `CommonUI.ShouldVirtualAcceptSimulateMouseButton` cvar.
If you return false, pressing the gamepad bottom button will only cause to trigger the real gamepad key, so no extra infinite left mouse button.

## Move from Game/All state to Menu in both ways.

A basic example is to have your root HUD be an activatable widget, set it active at your game stat and override the get UI config function to return a struct with the input mode set to All (or Game).

When your user does a specific action that activates a new widget, this widget will receive focus.
If later on you want to go back, meaning freeing the cursor so it can go anywhere in the viewport, you just have to deactivate it, since that will go back to your root HUD.