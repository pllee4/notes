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