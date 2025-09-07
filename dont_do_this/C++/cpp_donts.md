
- [Undefine Behaviour](#undefine-behaviour)
  - [return for \[\[noreturn\]\]](#return-for-noreturn)

## Undefine Behaviour

### return for [[noreturn]]

[source](https://youtu.be/p3ERaKsQmuU?si=04PqEuoWgj2KCFK7&t=350)

```cpp
[[noreturn]] constexpr int DontDoThis() { return 42;}
constexpr auto Execute() { return DontDoThis();}
std::cout << "Execute " << Execute() << std::endl; // segmentation fault
```