
# Get Config from request
You can get the config instance from a request handle by doing:
- `USmartObjectSubsystem::GetBehaviorDefinitionByRequestResult`
- from `UGameplayBehaviorSmartObjectBehaviorDefinition` get `GameplayBehaviorConfig` (c++ only)
- from there you can cast the config object to yours and get the variables you set there (if you had any variables bound to a parameter in the smart object definition, it will be set to that value)

**Example**:

Config: <br>
![[Pasted image 20260217182110.png]]


Definition: <br>
![[Pasted image 20260217182138.png]]


Component: <br>
![[Pasted image 20260217182202.png]]

Code (with custom config getter function): <br>
![[Pasted image 20260217182243.png]]