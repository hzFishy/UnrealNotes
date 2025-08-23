# Resources
- [Animation Budget Allocator](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-budget-allocator-in-unreal-engine)
- [Unreal Engine 5 Character and Animation Optimizations | Unreal Fest 2024](https://www.youtube.com/watch?v=N_suMyUuork)
- [Animation Debugging and Optimization](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-debugging-and-optimization-in-unreal-engine)
- [Rewind Debugger](https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-rewind-debugger-in-unreal-engine)
- [About Standard Blend, Dead Blending and Inertialization](https://github.com/Vaei/LocoTips/wiki/Inertialization)
- [Fast Path, Worker Threads, Performance](https://github.com/Vaei/LocoTips/wiki/Structuring:-Fast-Path,-Worker-Threads,-Performance)

# Quick Settings
- Disable `Tick Animation on Skeletal Mesh Init` in the Project Settings. This can cause hitches when initializing a lot of SKM at once. Disabling it will cause a T Poe for one frame, you can hide the SKM then show it once initialized.
