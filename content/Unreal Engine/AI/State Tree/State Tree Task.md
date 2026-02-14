
Here is my code template

```c++
// .h
#pragma once

#include "StateTreeTaskBase.h"
#include "%UNPREFIXED_CLASS_NAME%.generated.h"

USTRUCT()
struct FMyInstanceData
{
	GENERATED_BODY()
	
};

USTRUCT(DisplayName="Task")
struct %CLASS_MODULE_API_MACRO% %PREFIXED_CLASS_NAME% : public FStateTreeTaskCommonBase
{
	GENERATED_BODY()

	using FInstanceDataType = FMyInstanceData;

	virtual const UStruct* GetInstanceDataType() const override { return FInstanceDataType::StaticStruct(); }
};

```

