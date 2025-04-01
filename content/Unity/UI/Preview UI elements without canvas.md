
To render UI we need a canvas as a parent or root of the UI elements.

If you work on modular prefabs, this is an issue, because your `Button` prefab needs a canvas at its root to be previewed, and your `MainMenu` prefab needs a canvas for the same thing.
But we want to add our `Button` prefab in our `MainMenu` prefab and make it visible (because if you try you will see that the nested prefab will not render UI elements).

The fix is to remove the canvas element in your modular/nested prefab, and instead have a `Rect Transform` component at the root.
Unity will add a dummy canvas so you can preview your UI, like you can see below<br>
![[Pasted image 20250401141720.png]]