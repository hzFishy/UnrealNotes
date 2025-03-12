
By default you can't replicate a TMap, but you can by using a struct that warps your TMap.
[Here](https://gist.github.com/intaxwashere/505b3c59c452f86819e8e8622c3e96d7) is a code snippet of the `NetSerialize` function body you must implement in the struct.
*Thanks to Jambax (UE Source Discord)*
