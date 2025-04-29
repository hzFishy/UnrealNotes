
> [!Warning] Anim notifies are not guaranteed to be fired
> Good explanation by *Vaei*:
> 
> *"Don't use notifies for important events.
> A lot of the time they simply don't fire, you'll start seeing then when testing game in actual production. 
> Setting to branching point notify ensures they always fire.
> However branching point notifies don't fire if there are 2 on the same frame, especialy common when its the start of a montage.
> Use my task node/plugin instead, it basically looks for notifies of a specific type, caches the time where they should play, and starts a timer, and broadcasts an event"*
> 
> See [PlayMontageAdvanced](https://github.com/Vaei/PlayMontageAdvanced/)

