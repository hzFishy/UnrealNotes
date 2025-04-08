
## Const
by default the captured values are const, to make them mutable you can add `mutable` before `{}`.
Example:
```c++
.AddWeakLambda(this, [this, SafeInstanceData] () mutable { /* code */ }
```

## Capture
inside `[]`
- `=` captures `this` by value
- `&` captures `this` by reference
