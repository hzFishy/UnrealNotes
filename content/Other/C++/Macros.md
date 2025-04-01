
Macros parse "text" where they are used.

To avoid variable leaking (if the macros has at least one param) you should use `do` with `while` keywords the following way to lock the params in a scope (so they don't leak below).

```c++
#define dbgLOG(LogCategory, Msg, ...) do\
{\ 
   DBG::Log::Log(__COUNTER__, std::source_location::current(), LogCategory, DBG::Log::DbgLogArgs{}, TEXT(Msg) __VA_OPT__(,) __VA_ARGS__);\
}while(false)
```
