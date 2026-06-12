https://www.w3schools.com/datascience/ds_stat_intro.asp

`describe()`: summarize the data

```python
import pandas as pd

full_health_data = pd.read_csv("data.csv", header=0, sep=",")

pd.set_option('display.max_columns',None)
pd.set_option('display.max_rows',None)

print (full_health_data.describe())
```

---

Percentiles are used in statistics to give you a number that describes the value that a given percent of the values are lower than.

Task: Find the 10% percentile for Max_Pulse

```python
import pandas as pd
import numpy as np

full_health_data = pd.read_csv("data.csv", header=0, sep=",")

Max_Pulse= full_health_data["Max_Pulse"]
percentile10 = np.percentile(Max_Pulse, 10)

print(percentile10)
```
The 10% percentile of Max_Pulse is 120. This means that 10% of all the training sessions have a Max_Pulse of 120 or lower.

---

Standard deviation is a number that describes how spread out the observations are. A low standard deviation means that most of the numbers are close to the mean (average) value. A high standard deviation means that the values are spread out over a wider range.

```python
import pandas as pd
import numpy as np

full_health_data = pd.read_csv("data.csv", header=0, sep=",")

std = np.std(full_health_data)

print(std)
```

The coefficient of variation is used to get an idea of how large the standard deviation is.

> Coefficient of Variation = Standard Deviation / Mean

```python
import pandas as pd
import numpy as np

full_health_data = pd.read_csv("data.csv", header=0, sep=",")

cv = np.std(full_health_data) / np.mean(full_health_data)

print(cv)
```

---

Variance is another number that indicates how spread out the values are.

In fact, if you take the square root of the variance, you get the standard deviation. Or the other way around, if you multiply the standard deviation by itself, you get the variance!

```python
import pandas as pd
import numpy as np

health_data = pd.read_csv("data.csv", header=0, sep=",")

var = np.var(health_data)

print(var)
```

---

```python

```
