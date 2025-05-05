
- A `ref (&)` *is* a `pointer (*)` who by definition can't be null.
- A pointer holds a memory address.
- A pointer has a size of 64bits, it doesn't matter what type it is (this is why you can forward declare in header when you use a pointer).

This is why using `const ref` on types smaller than 64 bits is counter productive, and should not be done if the goal is to optimize and avoid a copy.
For example, if you use a `const ref` on a `int32` variable, you will actually double the size.