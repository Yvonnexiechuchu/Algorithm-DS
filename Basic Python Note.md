*This is a note for Python code that is widely used for data preparation and understanding between model building, I am consistently update the file throughout the whole learning process and practice.*  

*Latest update date:* ***2020-01-14***
***

#### Packages that are useful for predictive modeling
```python
import pandas as pd
from sklearn import linear_model
```

#### Load Dataset — Data Understanding
```python
basetable = pd.DataFrame('data.csv')
## return the number of rows in the dataframe
population_size = len(basetable)
## return the number of rows in the dataframe of a single column
targets = sum(basetable["Target"])
## count the number of occurrences of a certain value in a column
sum(basetable["variable"]==value)
```

#### Date Preparation
```python
## Define the variables
X = basetable[["predictor_1","predictor_2","predictor_3"]]`
y = basetable[["target"]]
```
