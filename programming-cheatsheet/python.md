# Common

## Pip
* Output installed packages
```
pip freeze > requirements.txt
```

## Conda environment
* Export and usage of environment.yml
```
conda env export > environment.yml
conda env create -f environment.yml
```

## Namedtuple
* Be extra careful when using set literal.
* The issue is that sets in Python are unordered collections, the field ordering could be inconsistent when set is passed to `namedtuple` as below
* Try to run few times and you would realize the order could be flipped

```
from collections import namedtuple
Vector = namedtuple("Vector", {"x", "y"})

vector = Vector(1.0, 2.0)
print(vector.x, vector.y)
```
* Fix: Use list
```
Vector = namedtuple("Vector", ["x", "y"])
```

## Walrus operator
* Error if is evaluated first
```
>>> results = [(value := slow(num)) for num in numbers if value > 0]
NameError: name 'value' is not defined
```

* Fix
```
results = [value for num in numbers if (value := slow(num)) > 0]
```

## Variable Positional Argument
[Item 22: Reduce Visual Noise with Variable Positional Arguments](https://www.amazon.sg/dp/0134853989)

```
def log(message, values):
    if not values:
        print(message)
    else:
        values_str = ', '.join(str(x) for x in values)
        print(f'{message}: {values_str}')
log('My numbers are', [1, 2])
log('Hi there', [])

def log(message, *values):
    if not values:
        print(message)
    else:
        values_str = ', '.join(str(x) for x in values)
        print(f'{message}: {values_str}')
log('My numbers are', 1, 2)
log('Hi there') # Much better
```

* only for situations where you know
the number of inputs in the argument list will be reasonably small.