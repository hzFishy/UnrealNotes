Extracted from [here](https://dev.epicgames.com/community/learning/tutorials/JXMl/unreal-engine-your-first-60-minutes-with-mass)


**Entity**
> This is the base class of Mass that holds pointers to all of its Fragments

**Fragment**
> Holds the data/state for an Entity in tightly packed arrays (e.g. transform, velocity, current LOD, etc.)

**Archetype**
> Entities with identical Fragment composition that are grouped together. Entity composition can change at runtime resulting in Archetype migration

**Trait**
> Group fragments together and typically represent Entity features (e.g. Movement, ZoneGraph Navigation, SmartObject User)

**Tags**
> Archetype-level, Dataless Fragments that can be used by Queries for filtering archetypes based on their presence or absence

**Chunk Fragment**
> Fragments that are applied to a subset of Entities in an Archetype instead of directly to a single Entity

**Shared Fragment**
> Fragments with data that are shared with multiple entities for memory optimization purposes

**Processor**
> Where logic execution occurs in Mass. Processors can change values of a Fragment as well as composition of Entities (adding or removing).

**Entity Query**
> A query used by processors that filters archetypes based on fragment and/or tag requirements. The EntityQuery returns batches of fragments without regard to the individual Entity identifiers

**Mass Spawner**
> System for adding Entities to a level at runtime

**Mass Entity Config** 
> Asset that defines the Mass agent to spawn by specifying traits of the Entity

