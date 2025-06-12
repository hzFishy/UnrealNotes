
(For demonstration purposes we assume you applied the shader material on a Image component but this works for anything)

For some reason you cannot simply do `myImage.material.SetXXX(shaderVarKey, myValue)`.

To edit a variable you have to
1. have a ref to your `Graphic` component (for example an `Image`)

2. implement `IMaterialModifier` on your mono script
3. implement `GetModifiedMaterial`
```c#
public virtual Material GetModifiedMaterial(Material baseMaterial)
{
	// protected Material _material;
    _material = new Material(baseMaterial);
    _material.SetFloat(progressHandle, currentProgress);
    return _material;
}
```

4. Each time you edit your script value (which is used in the `SetXXX` function, here `float currentProgress`) call `SetMaterialDirty` on your `Graphic` component.
