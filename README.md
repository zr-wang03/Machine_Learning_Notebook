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

Drop a role (a sample) (axis=0 => horizontal)

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
