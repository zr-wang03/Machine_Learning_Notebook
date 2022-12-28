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

* **Trends** - A trend is defined as a pattern of change.
  * `sns.lineplot` - **Line charts** are best to show trends over a period of time, and multiple lines can be used to show trends in more than one group.
* **Relationship** - There are many different chart types that you can use to understand relationships between variables in your data.
  * `sns.barplot` - **Bar charts** are useful for comparing quantities corresponding to different groups.
  * `sns.heatmap` - **Heatmaps** can be used to find color-coded patterns in tables of numbers.
  * `sns.scatterplot` - **Scatter plots** show the relationship between two continuous variables; if color-coded, we can also show the relationship with a third [categorical variable](https://en.wikipedia.org/wiki/Categorical\_variable).
  * `sns.regplot` - Including a **regression line** in the scatter plot makes it easier to see any linear relationship between two variables.
  * `sns.lmplot` - This command is useful for drawing multiple regression lines, if the scatter plot contains multiple, color-coded groups.
  * `sns.swarmplot` - **Categorical scatter plots** show the relationship between a continuous variable and a categorical variable.
* **Distribution** - We visualize distributions to show the possible values that we can expect to see in a variable, along with how likely they are.
  * `sns.histplot` - **Histograms** show the distribution of a single numerical variable.
  * `sns.kdeplot` - **KDE plots** (or **2D KDE plots**) show an estimated, smooth distribution of a single numerical variable (or two numerical variables).
  * `sns.jointplot` - This command is useful for simultaneously displaying a 2D KDE plot with the corresponding KDE plots for each individual variable.





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

<figure><img src=".gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

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

<figure><img src=".gitbook/assets/image (1) (2).png" alt=""><figcaption></figcaption></figure>



Use a swarmplot instead:

```
sns.swarmplot(x=insurance_data['smoker'],
              y=insurance_data['charges'])
```

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

Kernel Density Estimate plot:&#x20;

```
# KDE plot 
sns.kdeplot(data=iris_data['Petal Length (cm)'], shade=True)
```

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

2D KDE plot:

```
# 2D KDE plot
sns.jointplot(x=iris_data['Petal Length (cm)'], y=iris_data['Sepal Width (cm)'], kind="kde")
```

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Create color-coded plots:

* `data=` provides the name of the variable that we used to read in the data
* `x=` sets the name of column with the data we want to plot
* `hue=` sets the column we'll use to split the data into different histograms

```
# Histograms for each species
sns.histplot(data=iris_data, x='Petal Length (cm)', hue='Species')

# Add title
plt.title("Histogram of Petal Lengths, by Species")
```

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```
# KDE plots for each species
sns.kdeplot(data=iris_data, x='Petal Length (cm)', hue='Species', shade=True)

# Add title
plt.title("Distribution of Petal Lengths, by Species")
```

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

