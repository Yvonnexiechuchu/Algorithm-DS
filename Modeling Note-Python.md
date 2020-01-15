*This is a note for Python code that divided by different ML model. The note can be used as a cheat sheet during daily coding. I am consistently update the file throughout the whole learning process and practice.*  

*Latest update date:* ***2020-01-14***
***
### Logistic Regression
```python
## data preparation
from sklearn import linear_model
X = basetable[["predictor_1","predictor_2","predictor_3"]]`
y = basetable[["target"]]
## Create a logistic regression model logreg and fit it to the data.
logreg = linear_model.LogisticRegression()
logreg.fit(X, y)

## Assign the coefficients to a list coef instead of array
coef = logreg.coef_
for p,c in zip(predictors,list(coef[0])):
    print(p + '\t' + str(c))
    
# Print the intercept in the form of array
logreg.intercept_

```
