- [Undefine Behaviour](#undefine-behaviour)
  - [return for \[\[noreturn\]\]](#return-for-noreturn)
- [Bad Practice](#bad-practice)
  - [Disable Return Value Optimization (RVO or Named RVO)](#disable-return-value-optimization-rvo-or-named-rvo)

## Undefine Behaviour

### return for [[noreturn]]

[source](https://youtu.be/p3ERaKsQmuU?si=04PqEuoWgj2KCFK7&t=350)

```cpp
[[noreturn]] constexpr int DontDoThis() { return 42;}
constexpr auto Execute() { return DontDoThis();}
std::cout << "Execute " << Execute() << std::endl; // segmentation fault
```

## Bad Practice

### Disable Return Value Optimization (RVO or Named RVO)

[source: Item 25: Effective Modern C++](https://www.amazon.sg/Effective-Modern-Specific-Ways-Improve/dp/1491903996)

```cpp
Widget makeWidget()
{
   Widget w; 
   …
   return std::move(w); // NRVO as it's named local variable
}
```

```cpp
Object SomeFunction()
{
    Object object1;
    Object object2;
    return condition() ? object1: object2
// this would disable named return value optimization
// this is lvalue
}
```
- Do this instead
  
```cpp
    if (condition())
    {
        return object1;
    }
    else
    {
        return object2;
    }
```