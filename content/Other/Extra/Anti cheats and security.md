*Thanks to the Unreal Source Discord people*

> [!Info] Info
> This page mostly concerns and assume we are using Windows OS.

# Introduction

## Kernel

### Introduction
[Kernel](https://en.wikipedia.org/wiki/Kernel_(operating_system)) is a computer program running at the core of a operating system.

Simplified plan <br>
![[Pasted image 20250225211429.png]]

### Programs
When using `Run as administrator` you  are still in user mode.

You need to write a driver to make it runnable at kernel level. And kernel level has no code error tolerance, any issue is assumed to be critical and for safety reasons it throws a BSOD.

You don't have anything to accept to have a driver installed by a software, it is just a file copy to system folder (`C:\Windows\System32\drivers`) and registration.
If a driver sys was signed by a Microsoft approved key the OS will load it. If not, a warning message will show up, and you can choose to ignore the driver or run it.

See [this tutorial](https://learn.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/writing-a-very-small-kmdf--driver) on how to make a basic kernel program on Windows.
## Protection rings
[Protection rings](https://en.wikipedia.org/wiki/Protection_ring) are used to give to a program a set of permissions. 

For example, here are the rings for the `x86` available in protected mode. <br>
![[Pasted image 20250225212806.png|350]]

But most desktop OSs only uses 2 rings, r0 and r3 <br>
![[Pasted image 20250225212955.png|350]]


# Cheats

## Examples

### Aimbots
Modern soft aimbots already mimic human inconsistency.

### Anti recoil 

**A popular anti recoil hardware and software is `Cronus`**
> Cronus is a physical device that you plug your controller in and at the moment it's not detectable or bannable by any game as far as we know, even tho it's literally cheating.
> 
> It even comes with scripts to run on it for many popular games, like for Apex Legends it comes with anti recoil scripts* and there are even some that specifically try to boost the effect of aim assist.
> It's being sold online like a legit product.

*Apex legends has public static (non random) recoil patterns for every gun in the game. 

Anti recoil software knows what gun you are playing and uses that to know how to behave.

A solution would be to add some randomness in the recoil pattern, that a human wouldn't notice, keeping a skill game *(= good and legit player can learn the pattern)*.




## Miscs

**Trigger Bot**
A trigger bot is something common in cheat menus.
A trigger bot shoots for you if you aim at a target.



## Tools

> [!Danger] Warning!
> Please do not use these in multiplayer games, go for a solo game or your own one.
> I am listing them for research and testing purposes.

**Disassemblers**
- `Ghidra`
	- Shows you the disassembly for a Windows executable.
	- Can generated pseudocode.
- `IDA Pro`

**Other**
- Cheat Engine


# Anti cheats

## Types
There are different types of Anti Cheats.
- **Behavioural**: The anti cheat analyse the overall behaviour of the player, and makes a decision on that. Its also close to data science *(ex: "is the player doing insane 360s at a high amount?")*
- **Programmatic**: The anti cheat just do "maths", it sees if you killed a player through 5 walls, if you did a impossible angle shot. This is more hardcoded.

Behavioural is probably better than programmatic since bots are becoming more human-like.

With the increased difficulty to detect very well implemented cheats, AI Anti cheats are now a thing, since manual anti cheats are hard to implement and can be reverse engineered, meaning the dev team has to constantly update it to make it harder for cheat authors. But this costs time and money.
The idea is to analyse how legit player plays, and how cheaters plays to find the difference and ban only the cheaters. This could mean a algorithm constantly running, analysing, updating itself, reverse engineering known cheats to see how it works, and more.

Video about AI Anti Cheats: https://www.youtube.com/watch?v=LkmIItTrQP4.


## Considerations
It's really hard to tell the difference between an average player with a humanized cheating device and a good player.


## Software's
Vanguard, Easy Anti Cheat (EAS) and RICOCHET runs on the kernel level.

Vanguard seems more restricting since it asks for a reboot when its first installed or updated.
Easy Anti Cheats doesn't require a system restart, its seems that it runs on demand.

Games using EAC: Fortnite, Apex Legends, Rust and more
Vanguard is used by Riot Games.
RICOCHET is used on some specific COD versions.

Vanguard files are located in your EFI system partition. Right next to Windows boot img.
It looks like Vanguard is "hijacking" the Windows boot process from outside. Probably to be able to load the earliest.

## Reading RAM

Each app has a unique memory signature. The signature is a sequence of bytes.

If you simply try to read/write memory you will probably get strike by a anti cheat. Anti viruses do very similar behaviour/pattern scanning.

**How does this works ?**
> [!Quote]- Blue Man explanation
> Any time you want to read a process you need to get a handle to that process, you can basically intercept that handle request from a driver (`ObRegisterCallbacks`).
> 
> Any time anything tries to open a handle your driver will get notified from where you can block it depending on what process is trying to open it, etc...
> 
> Pretty much every single anti cheat will do this... but it's a game of cat and mouse, you have to make sure a cheat driver doesn't sneakily unregister your callback, swap it for its own, etc...
> 
> That's why Vanguard for example wants to load with the OS, to try and make sure it's the first thing to load before any cheating software can (they still can but more difficult) because it gives it a chance to protect itself from anything trying to manipulate it

**Known case #1**
RICOCHET was known for reading literal strings in the memory. It was simply searching memory for strings like "Trigger Bot". A bad actor could send you a chat message containing one of the strings and you'd get banned.
Article about it [here](https://cybernews.com/security/hacker-claims-to-have-banned-thousands-of-cod-players/).


**Known case #2**
League Of Legends had something similar a few years back, it scanned your browser tabs and if any of them matched "cheat engine" the game would close.

