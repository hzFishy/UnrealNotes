
See `FUIAction CopyAction` and `FUIAction PasteAction` in `SDetailSingleItemRow` and `SDetailCategoryTableRow`.

For example we will look for `SDetailSingleItemRow`.
The actions runs either `SDetailSingleItemRow::OnCopyProperty` or `SDetailSingleItemRow::OnPasteProperty`.

Those use `FPropertyEditorClipboard` functions such as `FPropertyEditorClipboard::ClipboardCopy` and `FPropertyEditorClipboard::ClipboardPaste`.
