
See `UMassEntityTraitBase`.
In `BuildTemplate` you can add fragments and cache values.
This is run when you press the "Validate Entity Config" button.

To use `AddSharedFragment` you must first create a shared fragment.
Here is a code snippet for that.
```c++
FMassEntityManager& EntityManager = UE::Mass::Utils::GetEntityManagerChecked(World);
FSharedStruct SharedFragment = EntityManager.GetOrCreateSharedFragment<FMassSimulationLODSharedFragment>(FConstStructView::Make(Params), Params);
```
If you want to pass constructor params see other usages in engine.
