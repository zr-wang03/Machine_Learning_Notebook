# Machine Learning: XGBoost

XGBoost stands for extreme gradient boosting, is an implementation for gradient boosting by tuning multiple features.&#x20;

```
from xgboost import XGBRegressor

my_model = XGBRegressor()
my_model.fit(X_train, y_train)
```

pararmeters:

<pre><code>n_estimator: maximum number of models to create during training # usually from 100 to 1000
<strong>
</strong><strong>early_stopping_rounds: how many rounds with no increase in validation scores should the model tolerate before stopping
</strong><strong>#To use this, you should also set aside some data for validation and feed that to the model during fitting
</strong><strong>
</strong><strong>learning_rate: a number to be multiplied to the result of each tree in order to avoid overfitting(usually 0.1)
</strong>
</code></pre>
