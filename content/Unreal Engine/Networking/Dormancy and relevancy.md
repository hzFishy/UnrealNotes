
# Dormancy
When an actor is set to dormant it shuts down its actor channel. (I am not sure how this behaves if using replicated actor component).

Without an actor channel no Server and Client RPCs can be sent/received.
NetMulticast RPCs can still be used because it doesn't need a actor channel. (Not sure if you can **call** and **receive** though).
You can also replicate properties on a dormant actor by calling `FlushNetDormancy` on the server.

**Why would you do that ? To lower network ticking**
As Kaos shared on the Unreal Source Discord: "even if you set \[the actors] to lowest net update \[the] prioritize actors \[logic] still gets called". So in a scenario where you have a lot of these actors, the tick time can be really impactful.

