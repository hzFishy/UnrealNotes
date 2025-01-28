Notes of practices I noted.

**Resources**
- [Setting up an Unreal Engine Studio the Epic Way | Unreal Fest 2024](https://www.youtube.com/watch?v=102O0FOEzNY)
- [P4 universal gam dev typemap](https://gist.github.com/jase-perf/3f6328fb66427802090f458775e481df)

# For Devs
- Setting up the workspace for the long run: [Post](https://dev.epicgames.com/community/learning/tutorials/8JYW/setting-up-an-unreal-engine-studio-the-epic-way) - [Video](https://www.youtube.com/watch?v=102O0FOEzNY)
- For version control:
	- Use Perforce if possible
		- if not, using Git efficiently for UE as a team: https://miltoncandelero.github.io/unreal-git
		- or try https://github.com/ProjectBorealis/UEGitPlugin
		- or have everyone install VS and [compile automatically](https://landelare.github.io/2022/09/27/tips-and-tricks.html#automatically-update-c-binaries)
## For new UE Devs
- Read the following: 
   - https://landelare.github.io/2023/01/07/cpp-speedrun.html
      - Regarding what you can DO with Live coding: [Link Imgur](https://imgur.com/a/what-can-i-do-with-live-coding-UEoRFWs)
   - Common issues answered here, no need to read all before trying stuff: https://tackytortoise.github.io/2022/06/24/common-slacker-issues.html
- Use `TObjectPtr` instead of raw ptrs for class members (`UObject`s only) for Engine 5.4 and above
   - More details [here](https://www.tomlooman.com/unreal-engine-cpp-guide/)

## Project setup
- Disable Widget Bindings (`Projet Preferences`->`Property Binding Rule`)

## Naming convention
- Prefix all classes by the project/game codename (1-5 letters) (**after the type prefix!**), this help knowing what is from the engine and what's from us.
   - for example if the codename is `PG`: `AMyActor` becomes `APGMyActor`
   - for example if the codename is `Kaos`: `AMyActor` becomes `AKaosMyActor`

- Delegates: 
   - Declaration of delegate ends with `Signature` (e.g: `F<CodeName>OnScoreChangedSignature`)
   - Declaration of property delegate ends with `Delegate` (e.g: `FOnScoreChangedSignature OnScoreChangedDelegate`)
   - Declaration of functions bonded to delegates ends with `Callback` (e.g: `void OnScoreChangedCallback(...)`)
- OnRep replicated variables:
   - `OnRep_FullVariableName()` -> Call `OnFullVariableNameReplicated`
   - `OnFullVariableNameReplicated()`
- RPCs function: 
   - `DoSomething_ServerRPC`
   - `DoSomething_ClientRPC`
   - `DoSomething_MulticastRPC`
- Headers for specific content:
  - For enums: `XXXEnum.h`
  - For struct: `XXXStruct.h`
  - For GameplayTags: `XXXGameplayTags.h`

## Folder Structure
- Define early how we split the source into folders, example: 
***
   - AI
      - ...
   - Core
      - GameMode
         - Base
            - `<ProjectCode>BaseGameMode` class
            - Components
               - `<ProjectCode><SomeName>BaseGameModeComponent` class
               - ...
         - InGame
            - ...
         - Lobby
            - ...
      - GameState
         - `<ProjectCode>GameState` class
         - Components
            - `<ProjectCode><SomeName>GameStateComponent` class
            - ...
   - Possessed
      - Base
         - BasePawn
            - `<ProjectCode>BasePawn` class
            - Components
               - `<ProjectCode><SomeName>BasePawnComponent` class
               - ...
         - BaseCharacter
            - `<ProjectCode>BaseCharacter` class
            - Components
               - `<ProjectCode><SomeName>BaseCharacterComponent` class
               - ...
         - BaseController
            - `<ProjectCode>BaseController` class
            - Components
               - `<ProjectCode><SomeName>BaseControllerComponent` class
               - ...
      - Player
         - PlayerCharacter
            - Base
               - `<ProjectCode>PlayerBaseCharacter` class <- Child of `<ProjectCode>BaseCharacter`
               - Components
                  - `<ProjectCode><SomeName>PlayerCharacterComponent` class
                  - ...
            - PlayerCharacterType1
               - ...
         - PlayerController
            - Base
               - `<ProjectCode>BasePlayerController` class
               - Components
                  - `<ProjectCode><SomeName>PlayerControllerComponent` class
                  - ...
            - InGame
               - ...
            - Lobby/MainMenu
               - ...
      - AI
         - Base
            - AIPawn
               - `<ProjectCode>AIBasePawn` class <- Child of `<ProjectCode>BasePawn`
               - Components
                  - ...
            - AICharacter
               - `<ProjectCode>AIBaseCharacter` class <- Child of `<ProjectCode>BaseCharacter`
               - Components
                  - ...
            - AIController
               - `<ProjectCode>BaseAIController` class <- Child of `<ProjectCode>BaseController`
               - Components
                  - ...
   - UI
      - HUD
         - Base
            - `<ProjectCode>HUD` class
            - Components
               - `<ProjectCode><SomeName>HUDComponent` class
               - ...
         - InGame
            - ...
         - Lobby/MainMenu
            - ...
   - Systems
      - InventorySystem
         - Data
         - UI
         - ...
      - EquipmentSystem
         - ...
      - ...
   - GAS
      - Abilities
      - Effects
      - Cues
      - ...
   - Data
      - Core
         - AssetManager
      - ...
   - Utility
      - ...
   - Objects
      - ...
   - WorldObjects
      - ...
***

- Structure the header and source file using `#pragma region SomeCategory` and `/*-------`[...]`SomeSubCategory`[...]`-------*/` to make navigation faster, and easy collapse/uncollapse example :
```c++
#pragma region Properties

protected:
	/*----------------------------------------------------------------------------
		Base job info
	----------------------------------------------------------------------------*/
	bool bJobIsFinished = false;
	FGameplayTag CurrentJobGT;

	/*----------------------------------------------------------------------------
		Location
	----------------------------------------------------------------------------*/
	FGameplayTag CurrentJobLocationGT;

	UPROPERTY(ReplicatedUsing=OnRep_JobLocationVolume)
	AJobLocationVolume* CurrentJobLocationVolume = nullptr;

	/* Job locations where the player can go into */
	UPROPERTY(BlueprintReadOnly)
	TArray<FGameplayTag> JobLocationsAccess;

#pragma endregion


#pragma region Defaults
public:
	APGPlayerCoreCharacter(const FObjectInitializer& ObjectInitializer);
	virtual void Tick(float DeltaTime) override;
protected:
	virtual void PostInitializeComponents() override;
	virtual void BeginPlay() override;
	virtual void PossessedBy(AController* NewController) override;
	virtual void SetupPlayerInputComponent(class UInputComponent* PlayerInputComponent) override;
#pragma endregion


#pragma region Interfaces
   // Implemented interfaces methods
#pragma endregion

// Other needed categories ...
```

## Readability
- Design systems before writing code (using for example `excalidraw`)
- Comment functions, variable and logic inside functions bodies so any dev can understand the code at first glance.
- Comment if on server/client/local if not in the name (see RPC naming convetion)
- Make a variable `const` if not meant to be modified

## We work with and for designers
- Add Tooltips of exposed variables and events for designers
- Beware of the `Visible` specifiers on `UPROPERTY` to avoid unexpected flooding
- Usage of `IsDataValid()` for BP compilation for sanity assurance
- Use `Categories` meta specifier to help selection of GameplayTags
- For any functions that needs to be exposed to designers, create a custom function:
   - if its a class node:
      - The function is prefixed with `K2_` (ex: `K2_GetSomeThing`)
      - Set a specific display name for the function (ex: "Get Something") to hide the prefix and make it more readable
   - if its a custom K2 node, check `UK2Node` class
- Use `UDeveloperSettings` to make project-wide settings, easier to define and change than using an Actor/Data Asset/Components/... in most cases
   - Example of a Inventory System Settings: <br>
		![[Pasted image 20241029005348.png]]


## VCS
- Ideally use perforce for BP and asset locking and safety
   - Perforce training: https://training.perforce.com/learn
- Commit per feature change and not a bunch

## Code checks
- Use [asserts](https://dev.epicgames.com/documentation/en-us/unreal-engine/asserts-in-unreal-engine) to check conditions that should never happen.

## Miscs
- Collisions Management
  - Have a authorative documentation for collision channels and profiles ([Example](https://imgur.com/a/PdOgzOn))
  - Always use profiles

# For Designers
## Naming convention (Asset & blueprint prefixs)
- Blueprints (prefix ends with `BP_`):
   - Actor: `ABP_`
   - Actor Component: `ACBP_`
   - Scene Component: `SCBP_`
   - Pawn: `PBP_`
   - Character: `CBP_`

   - Game Instance: `GIBP_`
   - Game Mode: `GMBP_`
   - Game State: `GSBP_`
   - Player State: `PSBP_`
   - Player Controller: `PCBP_`
   - HUD: `HUDBP_`

   - Widget: `WBP_`

   - Gameplay Abilities: `GABP_`
   - Gameplay Effects: `GEBP_`
   - Gameplay Cues: `GCBP_`

- Other Assets:
   - Level: `L_`
   - Static mesh: `SM_`
   - Material: `M_`
   - Material Instance: `MI_`

   - Input Mapping Context: `IMC_`
   - Input Action: `IA_`
   - Data assets: `DA_`

## Readability
- Comment nodes and groups of nodes so anyone can understand the code at first glance

# For Designers and devs
## Naming convention
- Define early how we structure the editor folders (the structure has a lot of similarities with the source code structure): 
   - Content
      - `<ProjectName>`
         - Game
            - Core
               - ...
            - Possessed
               - ...
            - Systems
               - ...
            - UI
               - ...
            - Assets
               - ...
            - ...
         - Levels
            - ...

## Misc
- Set an empty level as the startup map to gain in iteration when launching the project.