
By default you can't replicate a TMap.


**Method 1**<br>
By using a struct that warps your TMap you can replicate a whole TMap.
[Here](https://gist.github.com/intaxwashere/505b3c59c452f86819e8e8622c3e96d7) is a code snippet of the `NetSerialize` function body you must implement in the struct. (This method sends the whole array, **NO DELTA detection**)
*Thanks to Jambax (UE Source Discord)*


**Method 2**<br>
For a large TMap, another method is to have use a fast array.
Each entry of a FA struct holds a Key and its Value.
Using the events of the FA you can reproduce the TMap locally.

