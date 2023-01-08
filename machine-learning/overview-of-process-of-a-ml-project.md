# Overview of process of a ML project

Basic workflow:

[https://www.kaggle.com/code/startupsci/titanic-data-science-solutions](https://www.kaggle.com/code/startupsci/titanic-data-science-solutions)

From the link above:

1. Question or problem definition.
2. Acquire training and testing data.
3. Wrangle, prepare, cleanse the data.
4. Analyze, identify patterns, and explore the data.
5. Model, predict and solve the problem.
6. Visualize, report, and present the problem solving steps and final solution.
7. Supply or submit the results.

\


Personal Thinking:

Understanding the data

1. Import the libraries
2. Look at the data given and see which sets are numerical(continuous or discrete or ordinal), which are categorical,  and which are not numbers (Like names etc)
3. Make educated guesses on what correlates with the result and what doesn’t&#x20;
4. Use visualization to support and complete the guesses (assumptions)
5. Drop the features that are not contributing, or merging several into one that contributes better

\


Preparing for modeling

1. Completing the data using mean values, random values or combined etc.
2. Changing the data into numerical ones, like going from female+male to gender:1 or 0. (Most models work better with numbers so we want that to be the case)
3. Choose the model that works best in the scenario (Supervised learning? Unsupervised?)（Structured data or not?）
4. Compare available choices and pick the one with the highest score of confidence.&#x20;

\
\


A violin plot is more informative than a plain box plot. While a box plot only shows summary statistics such as mean/median and interquartile ranges, the violin plot shows the full distribution of the data. The difference is particularly useful when the data distribution is multimodal (more than one peak). In this case a violin plot shows the presence of different peaks, their position and relative amplitude.

\
