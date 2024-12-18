# Resources
- [Detailed explanation of State Trees (from Epic dev)](https://www.youtube.com/watch?v=YEmq4kcblj4) *(The content of this video was noted in [[State Tree#General view]])*
- [Talk on it (from a studio)](https://dev.epicgames.com/community/learning/talks-and-demos/yj09/unreal-engine-exploring-the-new-state-tree-for-ai-unreal-fest-gold-coast-2024) on it
- [Another one (from another studio)](https://www.youtube.com/watch?v=zovPQnq7ndE)
- [Your First 60 Minutes with State Tree](https://dev.epicgames.com/community/learning/tutorials/lwnR/unreal-engine-your-first-60-minutes-with-statetree)
- [More info about the issues and behaviors of state trees](https://jeanpaulsoftware.com/2024/08/13/state-tree-hell/)
# General view

> [!info] Note
> Most of the context of what's following is from the Unreal Fest 2024 videos. Meaning that some statements might be true only in 5.4 or higher

## What is the state tree composed of?
A state tree is a tree of states (with a hierarchy).
The intermediate states (called branches) acts like selectors, this helps data driven actions and abstraction.
The nest states (called leaf's) contains actions and activities

## States
### Active states
- When a given leaf state is selected, all its parent states are activated to.
- Things set up in a parent state are also available in all child states.
<br>

![[Pasted image 20241217225522.png]]

### State selection

![[Pasted image 20241217233128.png]]

<br>Child state selection behavior <br>
![[Pasted image 20241217233257.png]]

## Tasks
### What are tasks ?
- The behavior of a state is set up using tasks
- A task is a piece of logic that will be active when a state is activated, examples are:
	- Set tags or values
	- Claim resources
	- Play animation
	- Move to

### Active tasks
- Each state can contain multiple tasks, which are active while the state is active
- Tasks on all active states are running
- Intermediate states tend to have the configuration tasks
- Leaf states usually have the action tasks
- **Task competition drives state completion**

Useful default properties on task class
![[Pasted image 20241218145240.png]]

### Global tasks
- Tasks that run for the duration of the whole tree, this can be useful for:
	- Exposing data to the state tree
	- Configuring event listeners
	- Doing clean up at exit

### Tasks exposed variables
- To make a variable showed as `CONTEXT`, add the `Context` category.
- To make a variable showed as `IN` (for input), add the `Input` category.
- To make a variable showed as `OUT` (for output), add the `Output` category.
## Transitions

### In general
- On completion transitions
	- When a state completes, we **must** transition to a new state
	- Failure handling

- On event transitions
	- When an event occurs, we **may** transition to a new state
	- Can also test on every tick, but more expensive

- Transitions can be refined with conditions


### Transition hierarchy
- Transitions are triggered from leaf state towards the root
- A state is completed when any of the actives tasks complete
	- Transitions are looked from the state where the task completed towards the root


### Conditions
- Used for state's enter conditions and transition conditions
- Use data binding to provide data for conditions
- Users can create their own conditions
- Conditions are combined to expressions
	- AND & OR (no precedence)
	- Use indentation to create nested expressions


### Implicit transitions
1. When a tree is initially started, we transition to the root state
2. If a state does not provide completion transition, we look on the parent
3. If the parent does not have one, we'll transition to root
4. If all completion transitions fails we transitions to root

## Events
- Event can be gameplay tag and/or a struct
- Events can be used to trigger transitions
- Events can be used as conditions for entering a state
	- The event is captured on state enter and available for tasks
- Events are the most effective way to do transitions
- Payload access and capture is great for reactions
- Events can be send by external logic or tasks


## Data flow

### Property binding
You can bind properties in various cases.
![[Pasted image 20241217234344.png]]

**Categories**
- Input
	- Value UI is hidden
	- Assumed to be always bound
- Output
	- Value UI is hidden
	- Not bindable, but can be bound to
- Context
	- Value UI is hidden
	- Assumed to be always bound
	- Automatically binds to similar context data
	- Auto binding can be overridden

All other properties as bindable if set to editable.


### Data Sources

![[Pasted image 20241217234432.png|350]]
![[Pasted image 20241218095852.png|350]]

1. Context data
	- Data provided from the State Tree use context
	- Actor Component vs Smart Object can have different data provided
2. Tree parameters
	- Configuration data provided from when a specific State Tree is used
3. Global tasks (and Evaluators)
	- Globally activated
4. State Parameters
	- Current and earlier states
5. Tasks
	- Tasks before the current task
	- Enter conditions, Transitions a bit different


### Binding functions

> [!info] New in 5.5

- State Tree nodes that set property values
- Useful for:
	- Conversion functions
	- Combine values
	- Get value based another value

### Property references

> [!info] New in 5.4

- State Tree property references allows the binding to return a pointer to the data
- Allows writing back results
	- Tasks that modify data (counters, etc)
	- Use state parameters as blackboard
- Pass references to instance data outside the State Tree

## Composition
### General
- State can be built from other branches of State Tree in three different ways
	- Linked subtrees
	- Linked assets
	- Parallel State Trees
- Composition allows more reuse and more efficiently

### Subtrees and linked states
![[Pasted image 20241218101643.png|350]]
- Subtree states allow to create reusable State Tree branches (Kind of like functions)
- Linked states allow you to link to the subtree branches
- Linked states can pass parameters to subtree states
- Subtrees can access global data
 
### Linked asset states

> [!info] New in 5.4

- Linked asset states allow you to replace a branch of the tree with another asset
- Reconfigure State Trees
- Allow multiple designers to build the behaviour
- Events shared between nested trees

### Run parallel task

> [!info] New in 5.5

- Run parallel task allows you to run another State Tree along with the current State Tree
- Parallel tree is run as a task, so it will start after state selection
- Events are shared between main and parallel tree task
- Good for secondary states, like weapon selection

### Linked overrides
- The asset and parameters provided to **linked asset** and **run parallel task** can be overridden when the State Tree is used.
- This allows you to build template State Trees, where parts of the tree are configured per character.

## Using State Trees

### How to run a State Tree
- The off-the-shelf case is to use State Tree component
- Works on any actor
- The component has functions to start and stop the tree
- Create a State Tree using State Tree component schema

### State Tree schema
- Define a specific State Tree use case
- What external data is available?
- What kind of nodes can be used to build the tree?

![[Pasted image 20241218103233.png]]

### Extending State Trees

The users can create their own schemas, tasks and conditions to create their own building blocks to make State Trees.

![[Pasted image 20241218103443.png]]

### Using State Tree in your custom context

- The State Tree has been created to be easy to embed for different use case.
- `FStateTreeReference` handles asset selection and parameters
- `FStateTreeInstanceData` stores the current runtime data for a State Tree instance.
- `FStateTreeExecutionContext` is a temporary object used as a helper to tick a State Tree
- Does not require an actor to tick but needs an owner `UObject` in case new objects are created

## State Tree Debugger

You can find it at `Window -> Debugger`

- Traces the State Tree execution
	- See state changes on timeline
	- See log how the tasks and conditions were executed
- Break points
	- Task Enter/Exit
	- State Enter/Exit
	- On Transition
- Enable/Disable
	- States
	- Tasks
	- Transitions
- Force conditions state on/off

# Miscs

## Helpers
You can use custom colors for states to visualize better.
[Section with more details](https://dev.epicgames.com/community/learning/tutorials/lwnR/unreal-engine-your-first-60-minutes-with-statetree#makingthestatetreeeasiertovisuallydistinguish)
