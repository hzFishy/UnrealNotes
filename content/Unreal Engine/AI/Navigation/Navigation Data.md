
Also check [[Nav Mesh Registration & Generation]] and [[AI Navigation Types]].

# About
The nav data seems to be built using the `INavRelevantInterface` interface with `GetNavigationData`.
For example `UPrimitiveComponent` implements it, as well as the parent of `UNavModifierComponent`, `UNavRelevantComponent`.

# Classes

## Global overview
### Objects & Interfaces
```mermaid
flowchart TD
%%{ init : {'flowchart': {'nodeSpacing': 100, 'rankSpacing': 100, "curve" : "step"}} }%%
    INavRelevantInterface --> UNavRelevantComponent
    INavRelevantInterface --> UPrimitiveComponent
    UNavRelevantComponent --> UNavLinkCustomComponent
    UNavRelevantComponent --> UNavModifierComponent
    INavRelevantInterface --> ANavLinkProxy
```

### Other types
```mermaid
flowchart TD
%%{ init : {'flowchart': {'nodeSpacing': 100, 'rankSpacing': 100, "curve" : "step"}} }%%
    FNavigationModifier --> FAreaNavModifier
    FNavigationModifier --> FCompositeNavModifier
    FNavigationModifier --> FCustomLinkNavModifier
    FNavigationModifier --> FSimpleLinkNavModifier
    
    FNavigationRelevantData
    FNavigationElement
    FNavigationElementHandle
```


## Links
See [[Nav Links]]

## Modifiers
See [[Nav Modifiers]]
