# Machine Learning: Data Leakage

Data leakage describes the situation in which information about the target is somehow contained in the training data, this makes the model perform well on training sets or even validation sets but will have a significant decrease in terms of performance when you use it to make predictions.&#x20;



CASE 1: Say if we want to predict the house price and have a feature that is the average sell price of the houses in the neighbourhood. If the data was collected without any effect from the training & test set,(Say that the data was from 10 years ago and all the transactions we have in the set was in the past 5 years) then it would be fine. However if the average was based on data from the training or testing dataset, (Or in other words, if the average changes when we sell a house now), then there might be data leakage. (Consider the extreme case that there was only one house sold in the neighbourhood and such that the average price is the same as the price we are trying to predict for this house.)

