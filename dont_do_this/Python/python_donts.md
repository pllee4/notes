- [Chain assignment](#chain-assignment)

## Chain assignment
- Fancy/boolean indexing returns a **copy**, not a view — so chaining a second `[...]` onto it mutates the copy, not the original.

```python
import numpy as np

region = np.array([0, 0, 0, 0, 0])
mask = np.array([True, True, False, False, True])

region[mask][region[mask] == 0] = 1
print(region)
## [0 0 0 0 0]   <- unchanged, silent bug

region[mask & (region == 0)] = 1
print(region)
## [1 1 0 0 1]   <- fixed: single mask, single assignment
```

- For pandas, there would be warning of `SettingWithCopyWarning`

```python
import pandas as pd

df = pd.DataFrame({"x": [1, 2, 3]})

df[df["x"] > 1]["x"] = 99
print(df)
#    x
# 0  1
# 1  2
# 2  3      <- unchanged, warning fires

# Fixed: single .loc assignment
df.loc[df["x"] > 1, "x"] = 99
print(df)
#     x
# 0   1
# 1  99
# 2  99     <- correctly updated
```