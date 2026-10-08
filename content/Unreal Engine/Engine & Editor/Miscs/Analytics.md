
# Resources
- [In Game Analytics (Docs)](https://dev.epicgames.com/documentation/unreal-engine/in-game-analytics-for-unreal-engine)


# Types
- `Analytics` module (See `FAnalytics`)
- `AnalyticsBlueprintLibrary` plugin (See `UAnalyticsBlueprintLibrary`)
- Log Category: `LogAnalytics`

# Misc
## Section names
- `Analytics`: Default and for release (`UE_BUILD_SHIPPING` or argument `-RELEASEANALYTICS`)
- `AnalyticsDebug`: Argument `-DEBUGANALYTICS`
- `AnalyticsTest`: Argument `-TESTANALYTICS`
- `AnalyticsDevelopment`: Fallback if nothing set and not `UE_BUILD_SHIPPING`

Check `EAnalyticsBuildType` and `GetAnalyticsBuildType` for details

# Provider
The `ProviderModuleName` must match a module name that implements the `IAnalyticsProviderModule`.
