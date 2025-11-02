
# Types

## `FMassArchetypeData`
An archetype is defined by a collection of unique fragment types (no duplicates).
Order doesn't matter, there will only ever be one `FMassArchetypeData` per unique set of fragment types per entity manager subsystem.

## `FMassArchetypeHandle`
An opaque handle to an archetype.

## `FMassArchetypeEntityCollection`
A struct that converts an arbitrary array of entities of given Archetype into a sequence of continuous entity chunks. The goal is to have the user create an instance of this struct once and run through a bunch of systems. The runtime code usually uses `FMassArchetypeChunkIterator` to iterate on the chunk collection.
