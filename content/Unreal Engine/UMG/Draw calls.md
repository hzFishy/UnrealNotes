
Each container has different draw call count

- A VerticalBox or SizeBox or such do have 1 Draw call.
- An overlay uses 2, cause of z Sorting
- A Canvas.. uses 3.. up to 5.. depending on Anchoring, pivot and z sorting 

If you nest two Vertical Boxes.. you get 2 Draw calls 
Nest two Overlays.. you get 4 Draw calls 
Nest two Canvases.. you get 9-15 Draw Calls.. cause their Draw calls raise exponential

*thanks for BDC_Patrick*

