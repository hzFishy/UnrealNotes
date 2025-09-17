See [Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-slots-in-unreal-engine)


# Basic incomplete UpperBody + LowerBody example

Basic example of a Full Body + Upper Body setup
<br> ![[Data/Pasted image 20250518155347.png]]

Here in pink you can see the Full Body bones and in green the Upper Body bones playing their respective animations.
- The Full Body (pink) is playing the walk animation (this is why the legs are moving straight)
- The Upper Body (green) is playing the punching animation (this is why above the spline the body is doing a punch)
<br>![[Data/animslots 2.gif]]

> [!Info] Slots have a lot of use cases!
> For this simple example I am playing the walk and punch animation in the Animation Graph, but this works to if (for example) you play the punch animation montage from gameplay code.


# Complete FullBody + UpperBody + LowerBody example
In this example montages using the "FullBody" slot will completely override any upper/lower body slots.

![[Pasted image 20250914130521.png]]

![[Pasted image 20250914130538.png]]