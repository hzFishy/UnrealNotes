
*Code snippet from nav on the Unity Discord*
```c#
bool IsPointerOverUI()
{
	PointerEventData eventData = new(EventSystem.current);
	eventData.position = Input.mousePosition;
	List<RaycastResult> results = new();
	EventSystem.current.RaycastAll(eventData, results);
	return results.Count > 0;
}
```

