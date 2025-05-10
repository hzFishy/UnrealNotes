On any scene component you have a property named `Parent Socket`, you can enter a value from a dropdown if the parent of your selected scene component has any sockets (bones + sockets).


# Breakdown of the `Parent Socket` browser
This dropdown is built from `FBlueprintComponentDetails::CustomizeDetails` (search for `Parent Socket` to find it as this function handles other stuff to).

When you click on the browse icon it runs `FBlueprintComponentDetails::OnBrowseSocket`, which will push a combo box (`SSocketChooserPopup`) if the parent has any sockets.
The sockets are queried with `USceneComponent::QuerySupportedSockets()` inside the widget `Construct` function.

Once you click on a socket entry, `ChooseSocket` (in widget class) will be called, which calls the `OnSocketChosen` delegate which runs `FBlueprintComponentDetails::OnSocketSelection`.

# `QuerySupportedSockets` overrides
Using the type of the returned socket description you can know if its a "pure" socket or a bone. 
## `StaticMeshComponent`
It iterates all sockets of type `UStaticMeshSocket`.
