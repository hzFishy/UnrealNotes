# Resources
- [Detailed explanation of State Trees (from Epic dev)](https://www.youtube.com/watch?v=YEmq4kcblj4) *(The important parts of this video was noted bellow in [[State Tree#General view]])*
- [Talk on it (from a studio)](https://dev.epicgames.com/community/learning/talks-and-demos/yj09/unreal-engine-exploring-the-new-state-tree-for-ai-unreal-fest-gold-coast-2024) on it
- [Another one (from another studio)](https://www.youtube.com/watch?v=zovPQnq7ndE)

# General view

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

### Global tasks
- Tasks that run for the duration of the whole tree, this can be useful for:
	- Exposing data to the state tree
	- Configuring event listeners
	- Doing clean up at exit



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

## Property binding
![[Pasted image 20241217234344.png]]

## Data Sources
<br>
![[Pasted image 20241217234432.png|350]]
1. Context data
	- 