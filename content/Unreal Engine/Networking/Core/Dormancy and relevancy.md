
# Dormancy
When an actor is set to dormant it shuts down its actor channel (affecting the actor, its subobject's and the owned actor components and their subobject's.

Without an actor channel no Server and Client RPCs can be sent/received.
NetMulticast RPCs can still be used because it doesn't need a actor channel.
You can also replicate properties on a dormant actor by calling `FlushNetDormancy` on the server. This will wake up the actor, replicate the properties, then go back to the dormant state it was.

**Why would you do that ? To lower network ticking**
As Kaos shared on the Unreal Source Discord: "even if you set \[the actors] to lowest net update \[the] prioritize actors \[logic] still gets called". So in a scenario where you have a lot of these actors, the tick time can be really impactful.

If using **DORM_Initial** the actor will replicate once before going to sleep.
