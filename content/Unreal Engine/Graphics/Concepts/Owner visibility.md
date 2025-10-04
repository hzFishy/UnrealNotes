
# First Person case
In first person scenarios you want your first person character mesh to be visible only for your local player, and the "classic" third person character mesh to be seen by any other external view (other players or sequencer cameras for example).

In this example, `Owner No See` is enabled on the default SKMC and `Only Owner See` is enabled on the first person SKMC.
In red you have the "classic" third person character mesh and in blue your first person character mesh.

> [!Warning] Warning
> The camera mesh is explicitly set to not hidden in game for demonstration purposes.
> For the same reason I exaggeratedly moved the first person character mesh forward.

This is the current "real world" representation of both meshes.
The player only sees the first person mesh (the one with the camera on).

![[Pasted image 20251004200532.png]]

And if we play a sequence, we can see that only the third person character is visible.

![[Pasted image 20251004200557.png]]