Enable `Include Crash Reporter` in the project settings
If you want to have a more detailed call stack, you need to include the debug symbols in your build, to do so enable `Include Debug Files`.

More info on how to use symbols to read mini dumps in shipping builds while not shipping the debug symbols directly to the user. [Time code](https://youtu.be/qT3E--_px28?si=vX0wjiT_cddlJyEC&t=624) (good for testing in a small environment)

**Custom crash reporter and receive crash logs** <br>
If you want to make the user send the crash report to you (and not epic games (by default)), you can use a external tool that supports UE crash reporter (some example are Sentry, Bugsplat and Backtrace) [Time code](https://youtu.be/qT3E--_px28?si=_WZ_iDdrTVkycQp2&t=1152).