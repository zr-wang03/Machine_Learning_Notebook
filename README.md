# PRE-Machine Learning : Data

To use pandas:

```
import pandas as pd
```

To read a csv file with pandas & print a summay of the data

```
file_path='../dirc/data.csv'
housing_data=pd.read_csv(file_path)
housing_data.describe
```

See a list of all columns

```
housing_data.columns
```

Drop a row (a sample) (axis=0 => horizontal)

```
housing_data = housing_data.dropna(axis=0)
```

Take out a specifc column(the target price in this example)&#x20;

```
y = melbourne_data.Price //Note price is the name of the column as seen above
```

Selecting multiple (columns) features (still, the strings are names shown above)

```
housing_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
X = housing_data[melbourne_features]
```

Show the first few lines&#x20;

```
X.head()
```

scikit-learn => sklearn

use DecsionTreeRegressor in sklaern:

```
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model
melbourne_model.fit(X, y)
```

Make a prediction

```
predicted_home_prices = melbourne_model.predict(X)
```

Use mean aboslute error

```
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
```

Use train\_test\_split from scikit-learn to split up the dataset

```
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
```

Compare the following two:&#x20;

```
candidate_max_leaf_nodes = [5, 25, 50, 100, 250, 500]
minMae=float('inf')
best_candidate=0
for candidate in range(len(candidate_max_leaf_nodes)):
    max_leaf_nodes= candidate_max_leaf_nodes[candidate]
    curr=get_mae(max_leaf_nodes,train_X,val_X,train_y,val_y)
    if curr<minMae:
        best_candidate=candidate
        minMae=curr
    
# Store the best value of max_leaf_nodes (it will be either 5, 25, 50, 100, 250 or 500)
best_tree_size = candidate_max_leaf_nodes[best_candidate]
```

```
scores = {leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y) for leaf_size in candidate_max_leaf_nodes}
best_tree_size = min(scores, key=scores.get)
```

Avoid overfitting using validation sets:

first define a function that returns the error using a specfic number of max\_leaf\_nodes. Then iteratively try several candidates to choose the best one.&#x20;

```
def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
    model = DecisionTreeRegressor(max_leaf_nodes=max_leaf_nodes, random_state=0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)
    
    
candidate_max_leaf_nodes = [5, 25, 50, 100, 250, 500]    
scores = {leaf_size: get_mae(leaf_size, train_X, val_X, train_y, val_y) for leaf_size in candidate_max_leaf_nodes}
best_tree_size = min(scores, key=scores.get)
final_model = DecisionTreeRegressor(max_leaf_nodes=best_tree_size)
# fit the final model and uncomment the next two lines
final_model.fit(X, y) # We have made our choice on which parameter to use so
#we don't need to keep the validation set, and can push it into the model for more 
#examples.
```



Use random forest instead:&#x20;

```
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state=1)
forest_model.fit(train_X, train_y)
melb_preds = forest_model.predict(val_X)
print(mean_absolute_error(val_y, melb_preds))
```



Generate a csv file that includes predictions for submission:

```
output = pd.DataFrame({'Id': test_data.Id,
                       'SalePrice': test_preds})
output.to_csv('submission.csv', index=False)
```
