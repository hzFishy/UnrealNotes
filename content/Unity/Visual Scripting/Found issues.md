
- ==Variable & event names cannot be edited across all instances== (must be edited by hand)

- Wildcard pins type are not updated after connecting a known type

- A error can be reported from a nested call
	- Example :
		- Something calls `EventA`, `EventA` calls `EventB`
		- an error occurs in `EventB`
		- In the console the error can be reported at `EventA`

- ==Adding object variables to prefab is broken==
	- If you add a object variable to the prefab definition, already placed instances will not be updated (resulting in missing object variables)
	- same issue with default value

