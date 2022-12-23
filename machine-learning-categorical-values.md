# Machine Learning: Categorical Values

3 ways to deal with categorical values:&#x20;

\=>drop the whole column

1. Get a list of categorical values:

```
# Get list of categorical variables
s = (X_train.dtypes == 'object')
object_cols = list(s[s].index)

print("Categorical variables:")
print(object_cols)
```

\=>Ordinal coding: change them with numbers: 1,2,3,4.&#x20;

1. This may imply a ranking within the values. May work well when the values are "Sometimes""Often""Everyday" but might have a difficult time with "Red" "Blue" "Green"
2. Scikit-learn comes with a OridnalEncoder

```
from sklearn.preprocessing import OrdinalEncoder

# Make copy to avoid changing original data 
label_X_train = X_train.copy()
label_X_valid = X_valid.copy()

# Apply ordinal encoder to each column with categorical data
ordinal_encoder = OrdinalEncoder()
label_X_train[object_cols] = ordinal_encoder.fit_transform(X_train[object_cols])
label_X_valid[object_cols] = ordinal_encoder.transform(X_valid[object_cols])
```

\=>One-hot coding

1. We call those values without an intrinsic ranking as nominal variables
2. Use OneHotEncoder from Scikit-learn
   1. handle\_unkown='ignore' is to deal with the error that the validation set contains values different from the training set
   2. sparse=False makes sure that the return is in numpy array

```
from sklearn.preprocessing import OneHotEncoder

# Apply one-hot encoder to each column with categorical data
OH_encoder = OneHotEncoder(handle_unknown='ignore', sparse=False)
OH_cols_train = pd.DataFrame(OH_encoder.fit_transform(X_train[object_cols]))
OH_cols_valid = pd.DataFrame(OH_encoder.transform(X_valid[object_cols]))

# One-hot encoding removed index; put it back
OH_cols_train.index = X_train.index
OH_cols_valid.index = X_valid.index

# Remove categorical columns (will replace with one-hot encoding)
num_X_train = X_train.drop(object_cols, axis=1)
num_X_valid = X_valid.drop(object_cols, axis=1)

# Add one-hot encoded columns to numerical features
OH_X_train = pd.concat([num_X_train, OH_cols_train], axis=1)
OH_X_valid = pd.concat([num_X_valid, OH_cols_valid], axis=1)
```



