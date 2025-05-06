Example snippet
```lua
-- Methods of the GM
type Impl = {
	__index: Impl,
	
	_New: () -> GameManager,
	GetInstance: () -> GameManager,
	
	Init: () -> ()
}

-- Properties of the GM
type Props = {
	_state: number,
	_minigameManagerInstance: MinigameManager.MinigameManager
}

local GameManager: Impl = {} :: Impl
GameManager.__index = GameManager;
export type GameManager = typeof(setmetatable({} :: Props, {} :: Impl))

_singletonInstance = nil;

-- Make new instance
function GameManager._New()
	local self = setmetatable({} :: Props, GameManager)
	self._state = CoreTypes.GameState.None;
	return self
end

-- get instance
function GameManager.GetInstance()
	if not _singletonInstance then
		_singletonInstance = GameManager._New();
	end
	return _singletonInstance
end

function GameManager:Init()
	local instance: GameManager = self;
	_print("Initialized")
end

return GameManager
```
