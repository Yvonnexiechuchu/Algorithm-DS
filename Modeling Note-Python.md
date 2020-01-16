*This is a note for Python code that divided by different ML model. The note can be used as a cheat sheet during daily coding. I am consistently update the file throughout the whole learning process and practice.*  

*Latest update date:* ***2020-01-15***
***
## Variable Selection
*with many variables, model tend to be: 1) Overfitting 2) Hard to maintain or implement 3)Hard to inteprete, multi-collinearity*
### Forward stepwise 
```python

```
### CHI SQUARE
<https://github.com/Yvonnexiechuchu/Algorithm-DS/blob/master/Chi-square%20test%20note.md>

### Multicollinearity
The general rule of thumb is that VIFs exceeding 4 warrant further investigation, while VIFs exceeding 10 are signs of serious multicollinearity requiring correction.
```python
from statsmodels.stats.outliers_influence import variance_inflation_factor

def calculate_vif(features):
    vif = pd.DataFrame()
    vif["Features"] = features.columns
    vif["VIF"] = [variance_inflation_factor(features.values, i) for i in range(features.shape[1])]    
    return(vif)

vif = calculate_vif(features)
while vif['VIF'][vif['VIF'] > 10].any():
    remove = vif.sort_values('VIF',ascending=0)['Features'][:1]
    features.drop(remove,axis=1,inplace=True)
    vif = calculate_vif(features)
  
final_vars = list(vif['Features']) + ['target']
df_selected=df[final_vars]
```


## Partition the data in a train and test set.
```python
from sklearn.cross_validation import train_test_split
## Carry out 80-20 partititioning with stratification
X_train, X_test, y_train, y_test = train_test_split (X, y, test_size = 0.8, stratify = y)
## Create the final train and test basetables
train = pd.concat([X_train, y_train], axis=1)
test = pd.concat([X_test, y_test], axis=1)
```

## MODEL
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
    
## Print the intercept in the form of array
logreg.intercept_

## Make a prediction for each observation in new_data and assign it to predictions
predictions = logreg.predict(new_data)
predictions_probability = logreg.predict_proba(new_data)
```
### Random Forest

## MODEL EVALUATION
### AUC-ROC CURVE,  checking any classification model’s performance
<https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5>
ROC is a probability curve and AUC represents degree or measure of separability. 
```python
from sklearn.metrics import roc_auc_score
auc=roc_auc_score(true_y,predictions_proba_y)
##calculates the AUC of model that is built on a train set and evaluated on a test set:
auc_train, auc_test = auc_train_test(variables, target, train, test)
```
