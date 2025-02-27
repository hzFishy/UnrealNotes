
To filter the tags
```c++
UPROPERTY(meta=(Categories="Your.Tag.Category"))
```
You can also specify a list to combine multiple categories.

Other ways to filter:
- `UGameplayTagManager::OnFilterGameplayTag`
- `OnGetCategoriesMetaFromPropertyHandle`
