As Snaps was trying to make a UE project with almost nothing unnecessary running on tick, she realized that UE has a **lot** of plugins and features enabled by default. Which makes the editor and game (more or less) load and run slower.

[Here](https://github.com/daftsoftware/StarterProject) is a link to Snaps's lightweight project template repo. All plugins (except the essential ones that can be found in the `.uproject`) are **disabled**. As you use it, when you need something that's missing, find the plugin and enable it.

Please note that the template was made in 5.5 and works on Windows (so it might be broken to use on other platforms).
To use it for older or newer versions, you could just copy some of the entries and files in `Config/`, `Source/` and `.uproject`. 
For `Content/` you cannot use the assets in a older version. So a workaround would be to create a project in the target old version, and copy the same files (for example the Font, materials, ...). There is a world grid material override in `DefaultEngine.ini` that should be changed (or commented), the project will crash otherwise if not running in 5.5.

