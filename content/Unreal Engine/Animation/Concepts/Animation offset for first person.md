See also [[First Person Rendering]]
# Example in 5.6 First Person template
In the new 5.6 variant first person template, the player character blueprint uses 2 skeletal mesh.
This is an interesting approach if you have a first person character that can be seen by other players or which should be rendered differently depending on other things such as cinematics.

![[Pasted image 20251004192420.png]]

The default native one acts like a "classic" third person character and has an AnimBP with the animation states and so on.
The second SKMC (called `FirstPersonMesh`) has the same skeletal mesh asset but a different AnimBP.

**An important info is that `Owner No See` is enabled on the default SKMC and that `Only Owner See` is enabled on the first person SKMC**


The first person animation blueprint only does 2 things in the AnimGraph:

![[Pasted image 20251004192830.png]]

1. Copy the skeletal pose from the parent (`Mesh (CharacterMesh0)`)
![[Pasted image 20251004193018.png]]

2. Apply a control rig which moves some bones (such as the head and shoulders)
![[Pasted image 20251004192910.png]]


This gives the following result (I moved the components for better visibility):

![[Pasted image 20251004192708.png]]

The great thing with that is that you only have to set up your AnimBP once and procedurally update your first person one.
