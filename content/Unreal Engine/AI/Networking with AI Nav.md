

In a MP project if you play the game you might see the `Navmesh need to be rebuilt` error message.

To solve this, you either have to enable navigation on client side: `ProjectSettings->Allow Client Side Navigation`
Or you can automatically destroy the RayCastMesh: `ProjectSettings->Auto Destroy when No Navigation` or `Auto Destroy when No Navigation` individually on each actor.


