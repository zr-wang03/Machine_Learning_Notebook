# Machine Learning : Cross Validation

In order to  better represent model strength with validation scores, we want to make sure that we can use a large validation set. That is when cross validation comes in place. In cross validation, say we have 5 different models and we want to test how strong they are. We can separate the samples into 5 parts and for each model use one of the parts for validation and the rest for trainning. This way all of the samples are being used during training and validation.&#x20;

This may affect run time though so it is more appliable to a small dataset.&#x20;

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Use cross\_val\_score from sklearn

```
from sklearn.ensemble import RandomForestRegressor
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer

my_pipeline = Pipeline(steps=[('preprocessor', SimpleImputer()),
                              ('model', RandomForestRegressor(n_estimators=50,
                                                              random_state=0))
                             ])

from sklearn.model_selection import cross_val_score

# Multiply by -1 since sklearn calculates *negative* MAE
scores = -1 * cross_val_score(my_pipeline, X, y,
                              cv=5,
                              scoring='neg_mean_absolute_error')

print("MAE scores:\n", scores)
```



In pandas, the `step` parameter is an optional argument that can be passed to the `pipe` method when chaining together transformers in a pipeline. It specifies the step name or names of the transformers to be applied to the input data.

The `step` parameter can be a string, a list of strings, or a dictionary, depending on the transformers being used in the pipeline.

Here is an example of using the `step` parameter with a string argument

```python
def transformer_1(df):    
    # Modify df and return modified df
    return df

def transformer_2(df):
    # Modify df and return modified df
    return df

df_modified = df.pipe(transformer_1, transformer_2, step='transformer 2')
```

In this example, the pipeline consists of two transformers, `transformer_1` and `transformer_2`. The `step` parameter specifies that only the second transformer, `transformer_2`, should be applied to the input data.

Here is an example of using the `step` parameter with a list of strings:

```python
df_modified = df.pipe([transformer_1, transformer_2], step=['transformer 1', 'transformer 2'])
```

In this example, both transformers will be applied to the input data, in the order specified in the `step` parameter.

Finally, here is an example of using the `step` parameter with a dictionary:

```python
df_modified = df.pipe(transformer_1, transformer_2, step={'transformer 1': transformer_1, 'transformer 2': transformer_2})
```

In this example, both transformers will be applied to the input data, in the order specified by the dictionary keys.

The `step` parameter can be useful when you want to apply only a subset of the transformers in a pipeline, or when you want to specify the order in which the transformers should be applied. It can also make the code more readable by giving the transformers explicit names.
