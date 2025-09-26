# Resources
-  [Collision Data in UE5: Practical Tips for Managing Collision Settings & Queries | Unreal Fest 2023](https://www.youtube.com/watch?v=xIQI6nXFygA)
- See [Streaming Improvements for Dense Worlds in The Witcher 4 UE 5 Tech Demo | Unreal Fest Orlando 2025](https://www.youtube.com/watch?v=BdopUm1_1_E)
	- enable async chaos creation state (new in 5.6, `p.Chaos.EnableAsyncInitBody = true`)
	- [[Fast Geo]] (new in 5.6)

# Miscs
- Disable "Generate Hit" and "Generate Overlap" on primitive components if you don't need it.
- Set the correct `Collision Enabled` value on your primitive components.
- Set the correct ignore/overlap/block channels to gain simulation time.
	- For example, if you have a trigger box that is only used with the player, ignore all object types except the player object type (by default this is `Pawn`).
