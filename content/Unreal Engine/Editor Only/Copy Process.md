
When you copy actors `UUnrealEdEngine::CopyActors` is run. This generates a text and copies it to the clipboard.

Between the default editor gather & filtering and the text export `OnFilterCopiedActors` is broadcasted, allowing you to edit the array of actors that will be copied.
