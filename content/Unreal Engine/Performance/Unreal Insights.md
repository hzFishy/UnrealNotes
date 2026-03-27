
# Resources
- [zomg's page](https://zomgmoz.tv/unreal/Unreal-Insights)
- [Docs](https://dev.epicgames.com/documentation/en-us/unreal-engine/unreal-insights-in-unreal-engine)
- [Adding Counters & Traces to Unreal Insights & Stats System](https://tomlooman.com/unreal-engine-profiling-stat-commands/)

# Shortcuts
- Press `F` to focus your selected frame

Double click on an event will highlight all similar
<br>![[Pasted image 20250618232705.png]]

# Game thread named events
- `GameThreadWaitForTask`

# Add your custom event
Use `TRACE_CPUPROFILER_EVENT_SCOPE(MyAwesomeevent);` (comment says we shouldn't give a string)

