
# Resources
- [Plugin](https://github.com/Flassari/PSOCacheBuster) to clear PSO driver cache for testing, can also use `-clearPSODriverCache`
- [Setting up PSO Precaching & Bundled PSOs for Unreal Engine](https://www.tomlooman.com/psocaching-unreal-engine/)
- [Creating / Updating Bundled PSO Caches (script)](https://github.com/sinbad/UEScripts/blob/master/doc/BundledPSO.md)
- [PSO Precaching (Docs)](https://dev.epicgames.com/documentation/en-us/unreal-engine/pso-precaching-for-unreal-engine)
	- [Loading screen section](https://dev.epicgames.com/documentation/en-us/unreal-engine/pso-precaching-for-unreal-engine)
	- See `FShaderPipelineCache::NumPrecompilesRemaining`
- Also check [[Shaders]]


# Miscs

See `EPSOPrecacheMissType` (Type) and `EPSOPrecacheResult` (PSOPrecachingState) for more info about the PSO Cache miss info in the logs.

Engine functions:
- `FCompilePipelineStateTask::CompilePSO`
- `LogGeneralPSOMissInfo` and other `LogPSOMissInfo` functions (in PSOPrecacheValidation)

