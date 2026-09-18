# Resources
- [UI Performance & Invalidation Debugging | Unreal Fest Chicago 2026](https://www.youtube.com/watch?v=VxX1aah6TZM)
- [Using Simple Generic Materials to Improve UI Performance | Unreal Fest 2024](https://www.youtube.com/watch?v=-qo3ix-qqAE)
- [Optimizing User Interfaces](https://dev.epicgames.com/documentation/en-us/unreal-engine/optimizing-user-interfaces-in-unreal-engine?application_version=5.6)
- [Unreal Garden UI Performance](https://unreal-garden.com/tutorials/ui-performance/)
- [The UI Widget Hitch](https://dev.epicgames.com/community/learning/tutorials/6XW8/unreal-engine-the-great-hitch-hunt-tracking-down-every-frame-drop#theuiwidgethitch)
- [The Text Hitch](https://dev.epicgames.com/community/learning/tutorials/6XW8/unreal-engine-the-great-hitch-hunt-tracking-down-every-frame-drop#thetexthitch)
- [UI Invalidation](https://dev.epicgames.com/documentation/unreal-engine/invalidation-in-slate-and-umg-for-unreal-engine)

# Miscs
- [[Slate Draw calls]] 
- [[Slate Prepass]]
- Do not use widget sequencer animations for constant animations (use materials if possible)
- Check for widget [tick prediction](https://youtu.be/VxX1aah6TZM?si=0vnvRvKzRz2NEsMv&t=1539)
- Enable Volatile flag for widget changing almost each tick
