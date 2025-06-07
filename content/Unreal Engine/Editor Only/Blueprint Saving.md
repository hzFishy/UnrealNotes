
When you save a BP using Ctrl + S or by clicking the button you are executing a command that calls `FAssetEditorToolkit::SaveAsset_Execute` (binded in `FAssetEditorToolkit::InitAssetEditor`).

This function will get the Objects to save, as well as the associated packages.
It then calls `FEditorFileUtils::PromptForCheckoutAndSave` (This is where the prompt "what do you want to save" is managed if requested).

> [!Info] More info on the save prompt
> The prompt is created using `FPromptForCheckoutAndSaveParams`.
> 

After a lot of code and some filtering the array of packages to check out will be sent to `FEditorFileUtils::PromptToCheckoutPackagesInternal`.

---

But it seems that there is another road, which ends in `UPackage::Save2.`
`InternalPromptForCheckoutAndSave` -> `InternalSavePackage` -> `SaveAsset`
