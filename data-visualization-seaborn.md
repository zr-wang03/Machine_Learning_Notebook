# Data Visualization : Seaborn

Setup:

```
import pandas as pd
pd.plotting.register_matplotlib_converters()
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
print("Setup Complete")
```

To generate a lineplot:&#x20;

```
# Set the width and height of the figure
plt.figure(figsize=(16,6))

# Add title
plt.title("Daily Global Streams of Popular Songs in 2017-2018")

# Line chart showing how FIFA rankings evolved over time 
sns.lineplot(data=spotify_data)
```

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

Display a subset of the original data:&#x20;

```
# Line chart showing daily global streams of 'Shape of You'
sns.lineplot(data=spotify_data['Shape of You'], label="Shape of You")

# Line chart showing daily global streams of 'Despacito'
sns.lineplot(data=spotify_data['Despacito'], label="Despacito")
```



Create a bar plot: sns.barplot

```
# Bar chart showing average arrival delay for Spirit Airlines flights by month
sns.barplot(x=flight_data.index, y=flight_data['NK'])
```

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>



Creating a heatmap

```
# Set the width and height of the figure
plt.figure(figsize=(14,7))

# Add title
plt.title("Average Arrival Delay for Each Airline, by Month")

# Heatmap showing average arrival delay for each airline by month
sns.heatmap(data=flight_data, annot=True)
#annot=True is to keep every number on the cells
#otherwise there will only be color but not the numbers

# Add label for horizontal axis
plt.xlabel("Airline")
```

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Creating a scatterplot:

```
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'])
```

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Add another dimensional of data to it:

```
sns.scatterplot(x=insurance_data['bmi'], y=insurance_data['charges'], hue=insurance_data['smoker'])
```

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>



Add a regression line to it (Change to create a reg plot)

```
sns.regplot(x=insurance_data['bmi'], y=insurance_data['charges'])
```

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>



Add two regression lines to the case:

```
sns.lmplot(x="bmi", y="charges", hue="smoker", data=insurance_data)
# Note that this one is different in that we are only specificing the names but not 
# the columns.
```

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



Use a swarmplot instead:

```
sns.swarmplot(x=insurance_data['smoker'],
              y=insurance_data['charges'])
```

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
