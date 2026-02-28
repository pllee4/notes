- [Undefined Behaviour](#undefined-behaviour)
  - [return for \[\[noreturn\]\]](#return-for-noreturn)
  - [Iterator Invalidation](#iterator-invalidation)
- [Bad Practice](#bad-practice)
  - [Disable Return Value Optimization (RVO or Named RVO)](#disable-return-value-optimization-rvo-or-named-rvo)
  - [Calling Virtual Functions during Construction or Destruction](#calling-virtual-functions-during-construction-or-destruction)
  - [std::move on const object](#std-move-on-const-object)

## Undefined Behaviour

### return for [[noreturn]]

[source](https://youtu.be/p3ERaKsQmuU?si=04PqEuoWgj2KCFK7&t=350)

```cpp
[[noreturn]] constexpr int DontDoThis() { return 42;}
constexpr auto Execute() { return DontDoThis();}
std::cout << "Execute " << Execute() << std::endl; // segmentation fault
```

### Iterator Invalidation

```cpp
    std::vector<int> vec = {1, 2, 3, 4, 5};
    {
      auto it = vec.begin();
      
      // bug: push_back may cause reallocation, invalidating all iterators
      vec.push_back(4);
      vec.push_back(5);
      std::cout << *it; // undefined behavior
    }
    {
      for (auto it = vec.begin(); it != vec.end(); ) {
          if (*it == 3) {
              // vec.erase(it); // this would cause iterator becomes invalid
              it = vec.erase(it); // erase returns next valid iterator
          } else {
              ++it;
          }
      }
    }
```

* Remove-erase idiom

```cpp
    vec.erase(std::remove_if(vec.begin(), vec.end(), 
                            [](int x) { return x == 3; }), 
              vec.end());
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

* Do this instead
  

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

### Calling Virtual Functions during Construction or Destruction

[source: Item 9: Effective C++](https://www.amazon.sg/dp/0321334876)
* During base class construction, virtual functions never go down into derived classes.

```cpp
class Transaction { // Base class
public:
    Transaction() {
        logTransaction(); // This is the dangerous call in the constructor.
    }
    virtual void logTransaction() const {
        std::cout << "Logged a base Transaction." << std::endl;
    }
};

class BuyTransaction : public Transaction { // Derived class
public:
    virtual void logTransaction() const override {
        std::cout << "Logged a Buy Transaction." << std::endl;
    }
};

class SellTransaction : public Transaction { // Derived class
public:
    virtual void logTransaction() const override {
        std::cout << "Logged a Sell Transaction." << std::endl;
    }
};

int main() {
    // output as "Logged a base Transaction"
    BuyTransaction b;
    SellTransaction c;
}
```

### std::move on const object

```
const std::string str = "data";
auto moved = std::move(str);  // Copies, not moves

// Good
std::string str = "data";
auto moved = std::move(str);  // Actually moves
```

```
// This function can only copy, never move
void process(const std::string& str) {
    std::string local = std::move(str);  // Always copies
}

void process(std::string&& str) {
    std::string local = std::move(str);  // Moves
}
```
