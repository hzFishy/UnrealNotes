# Editor
The displayed menu for tags in tracks is `FObjectBindingModel`.
Inside `FObjectBindingModel::AddTagMenu` you can see how the tags are queried (`FObjectBindingTagCache::IterateTags`).

Inside `FObjectBindingTagCache::ConditionalUpdate` `UMovieScene::AllTaggedBindings` is called and iterated to fill the internal data this tag cache object maintains.

`ConditionalUpdate` is called from `FSequencer::Tick`.

The `UMovieScene` only holds a TMap of tags in `BindingGroups`.
It is only edited via the Binding functions such as `AddNewBindingTag` (which is not used in engine) or `TagBinding`.

When playing with `AddNewBindingTag` it doesn't show up in the tag list since it has no binding IDs as value in the TMap pair, but it will show up on the bottom horizontal list in the Tag Binding Manager (which isn't usable).

