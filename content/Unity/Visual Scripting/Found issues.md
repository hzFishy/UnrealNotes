
- ==Variable & event names are identified by literal string== (for variables user needs to type it manually or sometimes the dropdown will work)

- ==Variable & event names cannot be edited across all instances== (must be edited by hand)

- ==Events cannot return params and input count must be manually added==

- ==Most inputs of nodes cannot be directly typed in==, this means you need to add another node for the literal value

- Wildcard pins type are not updated after connecting a known type

- A error can be reported from a nested call
	- Example :
		- Something calls `EventA`, `EventA` calls `EventB`
		- an error occurs in `EventB`
		- In the console the error can be reported at `EventA`, with not indication in the call chain

- ==Adding object variables to prefab is broken==
	- If you add a object variable to the prefab definition, already placed instances will not be updated (resulting in missing object variables)
	- same issue with default value

- ==Broken node search==:
	- No contextual search
	- If dragging from pin, can hide possible nodes (I recommended always searching from nothing, in some simple cases like with bools, ints, floats or flow control it works)

- ==Weird parallelism for execution== *(its all events, not functions, so Unity doesn't wait for your code to finish before going to the next execution)*
	- I saw a checked value becoming false after a check IN THE SAME execution flow (in one case for now)
		- Details:
			- OnUpdate:
				- Check if `MyVar` is true, if true:
					- Do stuff
					- Do more stuff <<-- ERROR ! `MyVar` was now false (like if it was updated in a parallel process)
		- Solution: Each time i needed to do something that required `MyVar` to be true, i checked it before
	- In another case a game object returned from a collision event was becoming null after a single node execution (caching it fixed the issue)

- Nested graphs cannot have multiple output triggers (But its allowed to create multiple ones ?)

- Returned values from nodes aren't cached, meaning you need to save it or it will rerun it, or error (dynamic access error)

- ==No delegates support==

- ==Cursed and time consuming to make callable nodes from C# for graphs==

- Cannot diff properly

- And of course, bad UX