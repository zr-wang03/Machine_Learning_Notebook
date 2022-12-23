# Machine Learning: Missing values

First idea: drop the colomns with missing values.

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```
# Get names of columns with missing values
cols_with_missing = [col for col in X_train.columns
                     if X_train[col].isnull().any()]

# Drop columns in training and validation data
reduced_X_train = X_train.drop(cols_with_missing, axis=1)
reduced_X_valid = X_valid.drop(cols_with_missing, axis=1)
```

Second idea: Imputation(filling the empty values with some value: mean, medium, you say it)&#x20;

Use SimpleImputer from sklearn

NOTE: .fit\_transform() is equal to .fit().transform()

.fit() is used to compute the values: mean, medium and so

.transform() is using the values above to fill in the blanks

```
from sklearn.impute import SimpleImputer

# Imputation
my_imputer = SimpleImputer()
imputed_X_train = pd.DataFrame(my_imputer.fit_transform(X_train))
#The fit method is used on X_train so that the value properties in X_valid is not 
#reveal to the training set too early
imputed_X_valid = pd.DataFrame(my_imputer.transform(X_valid))

# Imputation removed column names; put them back
imputed_X_train.columns = X_train.columns
imputed_X_valid.columns = X_valid.columns
```

Building on that: add a new feature that records where imputation took place

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

```
# Make copy to avoid changing original data (when imputing)
X_train_plus = X_train.copy()
X_valid_plus = X_valid.copy()

# Make new columns indicating what will be imputed
for col in cols_with_missing:
    X_train_plus[col + '_was_missing'] = X_train_plus[col].isnull()
    X_valid_plus[col + '_was_missing'] = X_valid_plus[col].isnull()

# Imputation
my_imputer = SimpleImputer()
imputed_X_train_plus = pd.DataFrame(my_imputer.fit_transform(X_train_plus))
imputed_X_valid_plus = pd.DataFrame(my_imputer.transform(X_valid_plus))

# Imputation removed column names; put them back
imputed_X_train_plus.columns = X_train_plus.columns
imputed_X_valid_plus.columns = X_valid_plus.columns
```
