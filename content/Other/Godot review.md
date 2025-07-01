*thanks to duroxxigar on the Unreal Source Discord for writing this message after I asked about the following points in Godot (compared to Unity/UE):*
- Rendering
- QoL
- Data storage (DT/DAs ?)
- Multiplayer
- AI tools (pathfinding and so on)
- Gameplay framework (just comps ? or stuff like UE GM/GS/PS/PC/pawn)
- VCS support
- SFX/VFX tools and performance
- UI (is it shit like unity components or better like UMG)

---
**His answer:**
*Please note that duroxxigar knows and uses Godot since 2017 personally and at his studio for several years now*


**Rendering** is still one of its weakest areas, performance wise. Still has glaring performance problems with its GI solution. It also still has issues with shadows. Light baking is a fast process, compared to older versions. And it _feels_ faster than UE's light baking. It uses GPU to light bake and with UE - I've always had issues with the GPU doing the light bakes. The renderer isn't as customizable as Unity, but they are still working towards being able to replace the renderer entirely like you can do with the physics engine. Not 100% there yet, but that is the goal.

**Overall** - it is generally okay for a decent chunk of 3D games that indies are currently making and a vast majority of 2D games. You'll definitely run into some headaches before you typically would in UE/Unity. But they should still be surmountable. 

**QOL** - this is very subjective in my opinion, so take anyone's opinion with a huge grain of salt. Personally, I think it utterly fails at this in many areas. They try to pack virtually everything in the inspector inside of dropdowns. The other tools have really small windows, like the animation tree (which is analogous to the ABP) or the shader editor. You can make the shader editor a floating window now (if I recall correctly), but I think the animation tree is still stuck in a small window. Writing custom tools is generally decent though. Mainly because it is pretty much the exact same workflow as building a game and the approach to scripting in general is overall better than UE. In Godot, the workflow is expected for you to not have to go into C++, so more % of the API is exposed to the scripting layer. Overall, I do think UE has more polish for its editor and general workflow though.

**Data** storage has some quirks. You inherit from the `Resource` class. That is its version of scriptable objects or data assets. The main gotcha is that you can mark something as "unique to scene", meaning each instance of the scene gets its own copy. But that doesn't always work. So it can lead into strange bugs that aren't immediately apparent. On the C# side - there was a bug for some time where you would lose all of your resource references on each editor start. So you'd have to go and reset them every time you started the engine. Don't know if it has been fixed though. 

**Multiplayer** is extremely barebones. You're going to be writing pretty much everything yourself. It provides the ability to host/connect/rpc/network auth. That's pretty much it. In 4, they did introduce a multiplayer synchronizer node which kind of acts like UE's replication (without the onrep portion) but most people end up rolling their own because it is unsatisfactory. I think it just sends the field every frame. Regardless if it actually changed or not. Can't recall the exact specifics. In 3.x, if you used the most popular steam add-on and you were doing multiplayer, you couldn't do multiplayer in steam without rewriting your entire networking code because Godot had issues communicating with steam for some reason. I can't recall the exact details as this was like 6 years ago at this point. 4 should have fixed this, but I can't confirm. 
**01/07/2025 UPDATE**:
Now  have something like UE's basic variable replication. So, they've improved the multiplayer synchronizer. Pretty much, you can tell it to always replicate (even if nothing changes) or only when things change. And to emulate the OnRep style, just have a setter function.

**AI tools** - they don't really exist and never will exist according to Juan (reduz). Because he says that they are too game specific and can't serve the needs of everyone. If that sounds like it is bull crap, that is because it is. So you'll write your own or use a community plugin. They use Recast for navigation (like UE and pretty much every other available engine) and RVO for agent/nav avoidance. It is still a WIP from last I saw though and people are having issues mainly with dynamic obstacles (not agents).

**Performance** is generally okay though and they recently rewrote a lot of stuff and got things working better in a multi-threaded environment. 

No concept of a **gameplay** framework. You build everything yourself. Make scenes that are comprised of nodes. Scene will be a stand-in for your Blueprint or Prefab and nodes are you components. 

**VSC** - git is the preferred one. Works fine generally. They claimed to have fixed one of the more annoying parts of it where you get a lot of fake diffs because the scene file used to seemingly randomly change some stuff in the file itself, so it would create unnecessary clutter in the VCS. But can't confirm if it has been fixed. I have seen a lot less people complain about it however. 

**SFX** - I'm not the best when it comes to this in general. For me, as long as sound plays, I think it is good. But more audiophile type folks may not like how poor the tools are. You'd end up using a 3rd party solution anyway. Which is what most bigger studios do in Unity/UE (maybe not now with meta sounds?) anyway. But apparently the audio engine for Godot gets out of sync fairly easily. No idea what that means in the context of audio, but it is a very common complaint from people making audio focused games.

**VFX** - Writing shaders is pretty nice because it is very very approachable. I'm not a graphics guy, so I don't have much to talk about with this. Other than for stuff like particles - it is a lot of dropdowns. Generally, the editor is fairly compact so it can be a bit annoying with how much you will end up resizing some parts of the screen just so you can read/access some stuff. 

**UI** - generally one of the highlights of Godot. One of the things people love most about it. Better than Unity, slightly worse than UMG (mainly because of some of the advanced UI stuff that some people want to do). But for most use cases, I'd say it is pretty comparable to UMG. Others may disagree but I don't see much of a big difference honestly. At least in general usage.

Overall, for 3D - it is still pretty rough unless you want to build your own tooling. Then it is generally alright. Like, its skeleton IK stuff is still in a very rough spot after like 3 rewrites since 4.0. That said, this kind of stuff only matters based on the game that you're making. For 2D, it is generally pretty dang good.

