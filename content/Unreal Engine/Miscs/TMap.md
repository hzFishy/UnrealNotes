
you can safely remove entries while iterating a TMap IF you use a iterator
*thanks to Daekesh*
```c++
for (auto It = YourMap.CreateIterator(); It; ++It)
{ 
	It.RemoveCurrent();
}
```
