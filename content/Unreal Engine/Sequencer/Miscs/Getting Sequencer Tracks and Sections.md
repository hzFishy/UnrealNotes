
Very broad note for now, here is some working code to get an absolute 3d transform track which has a given tag in a given level sequencer.

```c++
// read the sequencer
	auto* HardSequencer = LevelSequence.LoadSynchronous();

	if (IsValid(HardSequencer))
	{
		const auto& FoundRes = LevelSequence->FindBindingsByTag("Sword");
		if (!FoundRes.IsEmpty())
		{
			auto SwordBindingGuid = FoundRes[0].GetGuid();

			if (const auto* MovieSceneBinding = HardSequencer->GetMovieScene()->FindBinding(SwordBindingGuid))
			{
				for (const auto& MovieSceneTrack : MovieSceneBinding->GetTracks())
				{
					if (auto* TransformTrack = Cast<UMovieScene3DTransformTrack>(MovieSceneTrack))
					{
						const auto& AllTrackSections = TransformTrack->FindAllSections(FFrameNumber(0));
						
						// get absolute section
						for (const auto& Section : AllTrackSections)
						{
							if (Section->GetBlendType() == EMovieSceneBlendType::Absolute)
							{
								auto* TransfromSection = Cast<UMovieScene3DTransformSection>(Section);
								if (TransfromSection)
								{
									const auto& Proxy = TransfromSection->GetChannelProxy();
									auto* TranslationChannelX = Proxy.GetChannel<FMovieSceneDoubleChannel>(0);
									auto* TranslationChannelY = Proxy.GetChannel<FMovieSceneDoubleChannel>(1);
									auto* TranslationChannelZ = Proxy.GetChannel<FMovieSceneDoubleChannel>(2);
									FVector Location;
									TranslationChannelX->Evaluate(0, Location.X);
									TranslationChannelY->Evaluate(0, Location.Y);
									TranslationChannelZ->Evaluate(0, Location.Z);
									return Location;
								}
								break;
							}
						}
						break;
					}
				}
			}
		}
	}
```
