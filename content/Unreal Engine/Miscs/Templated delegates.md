*Thanks to Blue Man ([Source message](https://discord.com/channels/187217643009212416/221799439008923648/1361485483518988408))*

**Code snippet**
```c++
struct FancyDelegates
{
	struct DelegateBase 
	{ 
		virtual ~DelegateBase() = default; 
	};
	
	template<typename T> struct TDelegate : public DelegateBase
	{
		using TFunction = std::function<void(T*)>;
		
		TFunction Function; TDelegate() = default;
		
		TDelegate(TFunction InFunction) : Function(std::move(InFunction)) { } 
	}; 
	
	
	template<typename T> std::function<void(T*)>& GetDelegate()
	{ 
		const std::string TypeName = typeid(T).name(); 
		
		if (auto It = Delegates.find(TypeName); It != Delegates.end())
		{
			return static_cast<TDelegate<T>*>(It->second.get())->Function;
		} 
		
		auto Delegate = std::make_shared<TDelegate<T>>();
		
		Delegates[TypeName] = Delegate; 
		
		return Delegate->Function; 
	} 
	
	template<typename T> void Subscribe(std::function<void(T*)> InFunction)
	{
		auto& Delegate = GetDelegate<T>();
		
		Delegate = std::move(InFunction);
	} 
	
	template<typename T> void Execute(T* InObject)
	{
		auto& Delegate = GetDelegate<T>();
		
		if (Delegate)
		{
			Delegate(InObject); 
		} 
	} 
	
	private: std::unordered_map<std::string, std::shared_ptr<DelegateBase>> Delegates; 
};
```
