
# Macros
The macros such as `DECLARE_MULTICAST_DELEGATE_XXX` and `DECLARE_EVENT`.

`DECLARE_EVENT` is considered as deprecated.

All what `DECLARE_MULTICAST_DELEGATE_XXX` really do is creating a typedef with your delegate name and params (`typedef TMulticastDelegate<ReturnType(Params)> MulticastDelegateName;`). Note that `ReturnType` is always `void`.

Example:
```c++
// delegate declaration with macro
DECLARE_MULTICAST_DELEGATE_OneParam(FPTAOnEquipmentSlotAddedSignature, UPTAEquipmentSlot* /* NewSlot */)

// the real generated code
typedef TMulticastDelegate<void(UPTAEquipmentSlot*)> FPTAOnEquipmentSlotAddedSignature;

```

# `TMulticastDelegate`
The default `UserPolicy` is `FDefaultDelegateUserPolicy`.
