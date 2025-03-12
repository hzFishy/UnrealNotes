
*Thanks to Blue Man (UE Source Discord)*


It covers global functions and member functions
```cpp
template <typename TReturnType>
struct TFunctionSize
{
    static constexpr size_t ReturnSize = sizeof(TReturnType);
};

template <>
struct TFunctionSize<void>
{
    static constexpr size_t ReturnSize = 0;
};

template<typename ReturnType, typename... Args>
struct TFunctionSize<ReturnType(*)(Args...)>
{
    static constexpr size_t FunctionSize = TFunctionSize<ReturnType>::ReturnSize + (sizeof(Args) + ... + 0);
};

template<typename ReturnType, typename ObjectType, typename... Args>
struct TFunctionSize<ReturnType(ObjectType::*)(Args...)>
{
    static constexpr size_t FunctionSize = TFunctionSize<ReturnType>::ReturnSize + (sizeof(Args) + ... + 0);
};


template <auto TFunction>
consteval size_t GetFunctionSize()
{
    return TFunctionSize<decltype(TFunction)>::FunctionSize;
}
```

**Results** <br>
![[Pasted image 20250312200154.png]] <br>
![[Pasted image 20250312200202.png]]