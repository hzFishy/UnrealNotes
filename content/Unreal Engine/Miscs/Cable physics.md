
# Fake (Line traces + ribbon)

[Video demo](https://youtu.be/-8IZLh5TDTM) from Greenosaur (Unreal Source Discord).

> [!Quote] From Greenosaur
> "for the line tension IIRC I just add a force every tick to the held item in the direction of the last segment, scaled by how far extended it is.
> The snagging itself is just line traces"

Illustration of the process<br>
![[Pasted image 20250328161332.png]]

# Real (Physics)
Use a cable Skeletal Mesh or a list of capsules with physic constraints.
