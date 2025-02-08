Thanks for the insights

VAT is a tech to render at a large scale multiple entities with animations.
Basically you sample a texture to move vertices in a vertex shader. Offsetting verts in a shader is much faster than updating them on the CPU. Its all shader work.

This tech doesn't work well if you want collisions, it's the biggest downside of it. Normally the vertex animation is visual only, and your collisions won't update. 
That being said GPU collisions are possible, just uncommon [https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-32-broad-phase-collision-detection-cuda](https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-32-broad-phase-collision-detection-cuda "https://developer.nvidia.com/gpugems/gpugems3/part-v-physics-simulation/chapter-32-broad-phase-collision-detection-cuda")

[Here](https://horugame.com/) is the website of a dev working on a RTS game handling more than 100k units.