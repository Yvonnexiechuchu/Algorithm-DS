*This is a note for Python code that is widely used for data preparation and understanding between model building, I am consistently update the file throughout the whole learning process and practice.*  

*Latest update date:* ***2020-01-17***
***

### Packages 
```python
import pandas as pd
```

### Load Dataset — Data Understanding
```python
## load data
df=pd.read_excel("data.xlsx")
df=pd.read_csv("data.xlsx")
basetable = pd.DataFrame('data.csv')

##return the summay of the dataframe
df.info()
df.describe()
df.column.value_counts()
df.dtypes

## return the number of rows in the dataframe
population_size = len(basetable)
## return the number of rows in the dataframe of a single column
targets = sum(basetable["Target"])

## count the number of occurrences of a certain value in a column
sum(basetable["variable"]==value)

## return the top or bottom row
df.head(1)
df.tail(1)

## sort the dataset using specific column, default by asc
df_sorted=df.sort(['target'])

## drop a specific column
df.drop('column name',axis=1, inplace=True)

## Display the first 5 rows of the columns after the first 5 columns
df.iloc[:,5:].head(5)

```
### Data visualization/DEA
```python
## corplot
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
corr = df.corr()
sns.heatmap(corr, 
        xticklabels=corr.columns,
        yticklabels=corr.columns)
```
### Date Preparation
```python
## Define the variables
X = basetable[["predictor_1","predictor_2","predictor_3"]]
y = basetable[["target"]]

## convert dependant variable to 1/0
df['target'] = df['y'].apply(lambda x: 1 if x == 'yes' else 0)

## fill the na in a column
df.fillna(0)
```

#### One-Hot Encoding
This process takes categorical variables, such as days of the week and converts it to a numerical representation without an arbitrary ordering. For example, if a data point is a Wednesday, it will have a 1 in the Wednesday column and a 0 in all other 6 columns.
```python
## One-hot encode the data using pandas get_dummies
## features are a data frame of all the X variables
features = pd.get_dummies(features)
# Display the first 5 rows of the last 12 columns
features.iloc[:,5:].head(5)

