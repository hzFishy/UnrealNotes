
# Introduction
When you need to use `U` macros in your c++ file you need to include the `xxxx.generated.h` header file and add `GENERATED_BODY()` inside the class/struct definition.


# Why include `xxx.generated.h` ?

When you compile your c++ code, UHT (Unreal Header Tool) will generate some code from the `U` macros (`UCLASS`, `UPROPERTY`, `UFUNCTION`, ...). This code is written inside the `.generated.h` header file. A second file (`.generated.cpp`) will include it.

# Why `GENERATED_BODY()` ?
It seems that this macro is a marker for UHT, so it knows where to paste some code inside the class/struct.

# To put it simply

When you press compile, the following happens:
1. UHT creates `.generated.h`  and `.generated.cpp` files when needed.
2. `U` macros are used as markers for UHT or replaced with pasted text.
3. Since the `.generated.h` file is included in your regular `.h` file the C++ compiler will compile your regular files with the UHT generated files.
