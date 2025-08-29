
*Thanks to Blue Man on the Unreal Source Discord for this high level view of the controversy around TAA and Nanite on UE*

**Explaining misinformation behind lumen, nanite, etc... coming from YT videos**
A lot of it stems from the dislike of TAA
TAA? Yeah does anyone actually like it? No
Is it a necessary evil? Absolutely, unless you want to go back 10 years into the past with the state of graphics
A lot of current modern rendering techniques rely on accumulation over multiple frames, piggybacking off of TAA, so no TAA is not this smudgy blur algorithm that they try to portray it as that people use because they feel like it

Now the problem is that most of the YT videos mix actual bits of truth that no one disagrees with, with lies and manipulation.
Some of them even admitting to having no professional experience with graphics programming.

A lot of misinformation comes from not understanding how nanite works [Discord message ?+1](https://discord.com/channels/187217643009212416/1090768182131826748/1319061346825797632) & [Discord message ?+2](https://discord.com/channels/187217643009212416/1090768182131826748/1319064515513417841 ) (from my own digging through the source code) and paper from siggraph if you actually want to learn about it https://advances.realtimerendering.com/s2021/Karis_Nanite_SIGGRAPH_Advances_2021_final.pdf

Now nanite has a baseline cost which everyone knows, which is much higher on older GPUs. Nanite will absolutely be slower with very simple scenes which they uses for their examples, which is where the manipulation part comes in and yes in those scenes you can outperform it with had made LODs. What they aren't telling you and their audience is that nanite scales sublinearly, meaning at a certain scene complexity nanite will outperform traditional methods and you pretty much can't beat it manually, sublinear scaling comes into play as the scene becomes more complex, there will be barely an increase in frame times, the time complexity would be constant if it wasn't for the auto cluster culling it has to perform.

They also don't realize that Nanite has 2 rasterization paths, hardware and software, software path is used to rasterize small triangles that would be inefficient for the hardware rasterizer, so quad overdraw that they are complaining about is actually not that big of an issue.

They also don't tell you that with nanite traditional draw calls are a thing of the past, nanite rasterizes all nanite geometry in a single draw call.
Traditional methods result (instancing helps but it can't compare because instancing works with the same instances of a mesh in the scene) in 1 draw call per every material in the scene, so a mesh with 2 material will be at least 2 draw calls, now another mesh in the scene using those 2 same materials results in yet another 2 draw calls.
Nanite completely eliminates that, you have 1 "global" draw call per unique material in the scene.
This reduction in draw calls is even more important than efficient triangle rendering.

With all that said SH2 remake didn't have a need to use nanite, they are using it for a few meshes only and paying the baseline cost.

Splotchiness with lumen mostly comes from devs not properly following the content requirement guidelines for lumen and you will find most of us aren't actually massive fans of Lumen.

Megalights are an experimental tech not yet recommended to actually be used, it allows you to have lighting scenarios and overlapping lights at no significant cost that would have previously not been feasible and yes that's actually the truth.
Removing overlapping lights **without megalights** has been a standard practice in game development in ages, he didn't show anything new there.
In general they show the very basics of optimization which everyone already knows about and uses but to audience that don't know it looks like they did something special.

They also deleted my constructive criticism I left on YT on multiple occasions and from different people that have criticism towards the claims presented in the videos.
Most of the videos are not trying to reach the devs that are supposed to be the target audience, they are creating fake outrage by manipulating their audience that are trusting them

And here is a video from other professionals in the industry "debunking" the claims https://fxtwitter.com/Aherys_/status/1869317392645423593

