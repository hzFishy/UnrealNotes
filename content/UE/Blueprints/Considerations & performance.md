
# General
- Blueprint nodes execution are slower than C++ code.
- Pure nodes are executed each time they are used (they don't cache the outputs like a impure node).
- Blueprint `For loop` is very slow because for each iteration it does a copy of the current entry.
- Inputs and outputs of nodes (pure, impure and Make/Break) are copied.


# Ressources
More details
- [Unreal Engine, and the hidden pitfalls of Blueprints](https://celdevs.com/unreal-engine-and-the-hidden-pitfalls-of-blueprints/)
- [Performance guideline for Blueprints and making sense of Blueprint VM](https://intaxwashere.github.io/blueprint-performance/)
