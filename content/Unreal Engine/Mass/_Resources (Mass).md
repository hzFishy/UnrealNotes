
# Resources
- [Large Numbers of Entities with Mass in UE5 | Feature Highlight | State of Unreal 2022](https://www.youtube.com/watch?v=f9q8A-9DvPo)
- [Mass Sample (Project + doc)](https://github.com/Megafunk/MassSample)
- [Your first 60 minutes with mass](https://dev.epicgames.com/community/learning/tutorials/JXMl/unreal-engine-your-first-60-minutes-with-mass)

# Notes
> [!Quote] MieskoZ
> "In Lego Fortnite Mass is driving all of the static/stationary actors - stuff you see around you like trees, rocks, sticks, bushes, whatnot - stuff you can interact with and/or destroy (dynamically switching from ISM when you get close or when it gets hit by physics). Also, some large rocks and cliffs you can walk on."

Mass handles LOD differently then other normal mesh
mass lets you do custom changes using LOD like "if LOD 0 we spawn a BP actor tree, if LOD 1 we use a ISM instance tree"
> [!Quote] MieskoZ
> "that's correct. Also, Mass's LOD calculations can be used for both visual aspects of an entity as well as its "simulation LOD"
