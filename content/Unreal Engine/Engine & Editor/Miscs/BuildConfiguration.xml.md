
See [Build Configuration](https://dev.epicgames.com/documentation/en-us/unreal-engine/build-configuration-for-unreal-engine)


To change used MSVC toolchain for UBT:
```xml
<?xml version="1.0" encoding="utf-8" ?>  
<Configuration xmlns="https://www.unrealengine.com/BuildConfiguration">
	<WindowsPlatform>
		<ToolchainVersion>14.38.33130</ToolchainVersion>
	</WindowsPlatform>
</Configuration>
```