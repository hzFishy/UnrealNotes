- [TypedGameplayTags Plugin](https://github.com/MaksymKapelianovych/TypedGameplayTags)
- [Declare & Define Native Gameplay Tags](https://unrealdirective.com/tips/declare-define-native-gameplay-tags)

# Filtering

To filter the tags
```c++
UPROPERTY(meta=(Categories="Your.Tag.Category"))
```
You can also specify a list to combine multiple categories.

Other ways to filter:
- `UGameplayTagManager::OnFilterGameplayTag`
- `OnGetCategoriesMetaFromPropertyHandle`

Also see [Hojo finding](https://discord.com/channels/871938221892833290/925078515399946252/1401952687632810055) to make less hardcoded gameplay tag categories for UPROPs.
See [helper type](https://github.com/HomerJohnston/Yap/blob/main/Source/Yap/Public/Yap/GameplayTagFilterHelper.h).

# Remapping
Thanks to Sharundarr on the Unreal garden discord.

![[Pasted image 20250806225105.png]]
