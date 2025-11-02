
# `FMassEntityQuery`

## About
`FMassEntityQuery` is a structure that is used to trigger calculations on cached set of valid archetypes as described by requirements. 
See the parent classes `FMassFragmentRequirements` and `FMassSubsystemRequirements` for setting up the required fragments and subsystems.

A query to be considered valid needs declared at least one `EMassFragmentPresence::All`, `EMassFragmentPresence::Any`, `EMassFragmentPresence::Optional` fragment requirement.

## Executing logic for found entities

**ForEachEntityChunk**
Runs on the same thread the lambda on all the chunks.

**ParallelForEachEntityChunk**
Will try to spread the lambda on the chunks on available threads.

> [!Example] How ParallelForEachEntityChunk spread the work on threads
> *Thanks to LeroyMackerl*
> "Say you have 10 available threads, and 2000 entities, let's assume there is 200 entities per chunk. It will spread the 10 chunks to the threads, each thread got 1 chunk to work with"
> 
> This means that if you only had 5 available threads, each thread will have to process 2 chunks, as the yall try to be run on the frame.

# `FMassFragmentRequirements`
Is a structure that describes properties required of an archetype that's a subject of calculations.

# `FMassSubsystemRequirements`
Is a structure that declares runtime subsystem access type given calculations require.
