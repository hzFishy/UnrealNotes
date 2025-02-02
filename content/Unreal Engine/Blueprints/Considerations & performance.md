
# General
- Blueprint nodes execution are slower than C++ code.
- Pure nodes are executed each time they are used (they don't cache the outputs like a impure node).
- Blueprint `For loop` is very slow because for each iteration it does a copy of the current entry.
- Inputs and outputs of nodes (pure, impure and Make/Break) are copied.
- BP doesn't get optimized when compiled, it merges neighbor connections.
- BP is an array of uint8 in the background
# Misc

- BP compiles into bytecode that runs on BP VM, with the ability to call into native code (blueprint callable, blueprint pure, etc... functions). Here "VM" is a glorified switch statement, see below screenshots.
- 
![[Pasted image 20250202224243.png]]
![[Pasted image 20250202224543.png]]


# Resources
More details
- [Unreal Engine, and the hidden pitfalls of Blueprints](https://celdevs.com/unreal-engine-and-the-hidden-pitfalls-of-blueprints/)
- [Performance guideline for Blueprints and making sense of Blueprint VM](https://intaxwashere.github.io/blueprint-performance/)
