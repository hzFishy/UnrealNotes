
# Resources
- [Convert FFrameTime and FFrameNumber To Seconds](https://store.algosyntax.com/tutorials/unreal-engine/convert-fframetime-and-fframenumber-to-seconds/)

Snippet to get sequence length
```c++
FFrameTime SequenceEndFrameTime = FFrameTime(SequenceAsset->GetMovieScene()->GetPlaybackRange().GetUpperBoundValue());
float SequenceLength = SequenceEndFrameTime / SequenceAsset->GetMovieScene()->GetTickResolution();
```
