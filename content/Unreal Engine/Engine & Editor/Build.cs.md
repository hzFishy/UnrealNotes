
# Set macro value

```cs
PublicDefinitions.Add("MYMACRO=Value");
```

Be sure to have a `ifndef` check where you define `MYMACRO` otherwise it can be defined multiple times and be overridden.

