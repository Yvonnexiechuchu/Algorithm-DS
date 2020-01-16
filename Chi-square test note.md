The chi-square test is a statistical test of independence to **determine the dependency of two variables** (often expressed in a contingency table). It shares similarities with coefficient of determination, R². However, chi-square test is only applicable to **categorical or nominal data** while R² is only applicable to numeric data.

 We want to chose variable that has higher dependency. So, the way of using it in a feature selection is to run the chi-square test on every independent variable and the dependent variable, if there shows dependency, then we select the variable. On the contract, discard the X if it show no dependency.

For continuous variables, chi-square can be applied after "Binning" the variables.

*Degree of Freedom*
the maximum number of logically independent values, which have the freedom to vary. 

*Limitations*
Sensitive to small frequencies in cells of tables. 
Generally, when the expected value in a cell of a table is **less than 5**, chi-square can lead to errors in conclusions.

```python
## label encode the data if necessary - change the nominal or categorical value into 1/0 or 1/2/0
from sklearn.preprocessing import LabelEncoder
label_encoder=LabelEncoder()
df['column']=label_encoder.fit_transform(df['column']

from sklearn.feature_selection import chi2

## getting the X and Y before running the chi-square test
## chi2() will return: 
## 1st array-chi square values. the scores are better if greater. 
## 2nd array- p-value
chi_scores = chi2(X,y)
