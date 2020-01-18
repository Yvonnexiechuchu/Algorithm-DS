*This is a note for Python code that divided by different ML model. The note can be used as a cheat sheet during daily coding. I am consistently update the file throughout the whole learning process and practice.*  

*Latest update date:* ***2020-01-18***
***
## Variable Selection
*with many variables, model tend to be: 1) Overfitting 2) Hard to maintain or implement 3)Hard to inteprete, multi-collinearity*
### Forward stepwise 
Python's statsmodels doesn't have a built-in method for choosing a linear model by forward selection. 
I am trying to write one based on adjusted R-squared by adding features one at a time.
This part will be posted soon.

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
## data preparation
from sklearn import linear_model
X = basetable[["predictor_1","predictor_2","predictor_3"]]`
y = basetable[["target"]]

from sklearn.cross_validation import train_test_split
## Carry out 80-20 partititioning with stratification
X_train, X_test, y_train, y_test = train_test_split (X, y, test_size = 0.8, stratify = y)
## Create the final train and test basetables
train = pd.concat([X_train, y_train], axis=1)
test = pd.concat([X_test, y_test], axis=1)
```

## MODEL
### Linear Regression  
```python
from sklearn.linear_model import LinearRegression
linreg = LinearRegression()

linreg.fit(X_train, y_train)

feature_cols = ["predictor_1","predictor_2","predictor_3"]
# pair the feature names with the coefficients
# hard to remember the order, we so use python's zip function to pair the feature names with the coefficients
zip(feature_cols, linreg.coef_)

```
### Logistic Regression
```python

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

### SVM Support Vector Machine
Can be used to solve both classification and regression problems, but I mainly focused on classification first
```python
from sklearn import svm
## We set a SVM classifier, the default SVM Classifier (Kernel = Radial Basis Function)
classifier = svm.SVC(kernel='linear')
classifier.fit(X_train, y_train)

prediction = classifier.predict(X_test)
## using a confusion matrix to show the result
cm = confusion_matrix(y_test, prediction)
```
### Random Forest
Reference<https://towardsdatascience.com/random-forest-in-python-24d0893d51c0>
For classification and Regression task, using different part from sklearn
Each tree in the forest is trained on a random subset of the data points with replacement (called bagging, short for bootstrap aggregating).
#### Regression
```python
## Regression
from sklearn.ensemble import RandomForestRegressor
## Instantiate model with 1000 decision trees
rf = RandomForestRegressor(n_estimators = 1000, random_state = 42)

rf.fit(train_features, train_targets)
## predict on the test data
predictions = rf.predict(test_features)
```
#### Classification
```python
from sklearn.ensemble import RandomForestClassifier
clf = RandomForestClassifier()
clf.fit(features_train,label_train)
```

get the importance of each variable/feature in the RF model  

- we could use the random forest feature importances as a kind of feature selection method.
```python
## Get numerical feature importances
importances = list(rf.feature_importances_)
# List of tuples with variable and importance
feature_importances = [(feature, round(importance, 2)) for feature, importance in zip(feature_list, importances)]
# Sort the feature importances by most important first
feature_importances = sorted(feature_importances, key = lambda x: x[1], reverse = True)
# Print out the feature and importances 
[print('Variable: {:20} Importance: {}'.format(*pair)) for pair in feature_importances];
```


## MODEL EVALUATION  
**MAE** is the easiest to understand, because it's the average error.  
**MSE** is more popular than MAE, because MSE "punishes" larger errors.  
**RMSE** is even more popular than MSE, because RMSE is interpretable in the "y" units. Easier to put in context as it's the same units as our response variable

FOR MODEL EVALUATION IN CLASSIFICATION MODEL: <https://www.ritchieng.com/machine-learning-evaluate-classification-model/#>  
### AUC-ROC CURVE,  checking any classification model’s performance
<https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5>  
ROC is a probability curve and AUC represents degree or measure of separability. 
```python
from sklearn.metrics import roc_auc_score
auc=roc_auc_score(true_y,predictions_proba_y)
##calculates the AUC of model that is built on a train set and evaluated on a test set:
auc_train, auc_test = auc_train_test(variables, target, train, test)
```

