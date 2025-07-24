
*Thanks to Blue Man*
 
```cpp
int a{};
int b{};

auto Blah = [a, &b](int Whatever)
{
  std::cout << a + Whatever << std::endl;
};

Blah(20);
```

Gets compiled as something like
```cpp
struct SomeCompilerGeneratedName_Lambda
{
  void operator()(int Whatever) const
  {
     std::cout << a + b + Whatever << std::endl;
  }
  
private:
  int a;
  int& b;
};

int a{};
int b{};
SomeCompilerGeneratedName_Lambda Blah{a, b};

Blah.operator()(20);
```
