
On mobile the shader graph Time node has issues, the easiest fix is to make a mono script and [manually write](https://docs.unity3d.com/Packages/com.unity.ugui@3.0/manual/HOWTO-ShaderGraph.html#pass-custom-data-into-the-shader) the `Time.time` value on a float property.

