# SC1015MiniProject
SC1015 Mini Project - Mahesh Nithilan, Rajkumar Saanvi, Sunilkumar Hrishikesh
## Introduction
We are NTU students Nithilan, Hrishikesh and Saanvi here to elaborate on our SC1015 Mini Project.

## Practical Motivation
We wanted to be able to predict stock prices for the near future using Machine Learning and Data Science. 
Doing this would allow us to make substantial financial profit if implemented correctly and accurately for obvious reasons.
We chose not to do the same with cryptocurrencies because of their high volatility and tendency to sway to the market.

## Sample Collection
We first needed a way to collect the names and "tickers" of all publically traded companies whose stocks can be traded. 
We specifically chose NYSE because it is the biggest and oldest stock exchange globally.
We did this by finding an automatically updating list of NYSE stocks and tickers available on github, specifically from this https://github.com/rreichel3/US-Stock-Symbols/tree/main/nyse 
After this, we used the yfinance python module to collect the available day-to-day pricing data from yahoofinance for however long the company has been publicly available. However, this data contains the limitation that it does not have intra-day pricing information for all of this time.
Instead, we can access the daily stock price highs, lows, closing and opening prices as well as the divident and split adjusted stock closing prices.
We then randomly sampled the closing price information(the price at which the stock stops at when the stock market closes for the day) for 5 different companies and observed the trend in data, 
a sample of which can be shown here

## Problem Formulation
Predicting future short-term price movement and resultant prices using past inter-day data for NYSE publically traded companies through time-series regression

## Data Preparation
Given that we had prepared data collection and sampling from the prior stages, here we just had to decided what companies we wanted to develop our model based on. 
The data itself was very structured. We already only wanted closing data and so we seperated this from the other available data to decrease noise.
We just had to remove NA values because we mass downloaded our stocks and they were automatically fit to the earliest data available, which usually belonged to the oldest company
An example of this is shown below.

## Statistical Description
The description of the data provided some understanding of the variance and distribution of each stock, as well as the number of available data points. 
This latter statistic was useful in determining the range of training and testing data later on. The variance also allowed us to guage the relative volatility of the stock as a whole

## Exploratory Analysis
The formula for volatility is merely the standard deviation of the prices, and this was available with the simple .describe() function in pandas. 
We also computed the number of up-days and down-days for each stock. This is different from the overall price growth because we are only interested in day-day price changes and so knowing the ratio of ups-downs is highly useful

## Pattern Recognition
Because of the apparant randomness of the data, it was difficult to discover stock-specific trends just looking at the graphs visually or through simple regression models. We used the above formula to calculate the up-down ratio. 
What we also realized were these stocks were growing rather consistently, especially those in the S&P 500, with some small variations from y-y but overall increase in stock price. This made us realize that 
we were likely to be more successful in predicting stock prices if we chose less volatile stocks, so we limited ourselves to the S&P 500 Constituent Stocks. 

## Analytic Visualization
The best way of representing the information we chose is as a stock price chart as we've shown here. 

## Machine Learning
As we've already established, the movement of pricing data is naturally dynamic and highly sporadic day-to-day. With our goal being to predict the next days price, the most natural thing to do
 would be to take the previous days and try to see if theres some inner pattern
However, this inner pattern is very, very difficult to find through trial and error - if there even is one. Needing to compute this for every seperate stock we take is impossible and a waste of time. 
For this reason, and given the amount of data we had, we decided to use Neural Networks to approximate the stock price function for a specific stock.
The question then became what flavour of neural network to choose. Through research, we decided to use RNN implementations because they were perfect for time series data. The specific implementation we 
chose
of RNNs were LSTMs(Long-Short Term Memory). This is because they avoided the vanishing/exploding gradient problem and are more capable of determining long term patterns because of their ability to 
selectively remember data deemed important. This filters alot of noise, which is crucial in this sporadic application. 

## Algorithmic Optimization
Implementation specifically, we starting by implementing a 3-LSTM Layer NN that takes one days price as input and is required to predict that price some n days later, lets call this the prediction buffer.
 
Through trial and error and research, we optimized the NN to use the adam optimizer and set the loss function to MSE, as the size and magnitude of the error is signficant in determining the new weights 
and biases.
Each LSTM Layer had 200 nodes, which we set because of the sheer magnitude of the data. 
The other implementation details are in the notebook, and each iteration of the model takes only a couple of minutes to train. However, it must be added that this method of trial and error in optimizing 
the hyperparametrs is suboptimal.
We then ran the algorithm on randomized stock selections to iteratively improve the implementation for a more general train-and-use application. We selected an 75-25 training-test buffer for viewing the 
variation in its entirety.
We ran for 30 epochs with a 30 batch size - to represent monthly data period updates


## Statistical Inference and Information Presentation
Now onto the results. We have two different characteristics we aim to be successful in. One is identifying the price movement, the other the price magnitude. 
We would also like to understand how well our model fits the training and test stock data, and the rate of error.

First, the price movement. We simply created a function that determined whether or not the direction of the predicted price was the same as the actual price for a given day. 
We then summed the number of instances and divided it by the number of predicted values to give us the price movement accuracy percentage. We then made a confusion matrix for the predicted values
in the test set for a given for a given stock

Secondly, we have the difference in magnitude between the predicted prices and the actual prices in the testing set. We provided both a graph and a summary for this. 
The graph consists of the direct percentage difference between the actual value and predicted price. The summary is in the form of MSE, to completely emphasize larger errors since they are more 
important.
We also tried testing the overall 3 day and week long error of the model if it used its own predictions to forecast future data. These graphs can be shown here.

Third, we wanted to look at the training loss and accuracy of our model as a stock function for different stocks, and we used the training output in Keras and R^2(or Explained Variance) of our models 
for the training and test sets

Fourth, we wanted to explore the correlation between the volatility of the stock computed before(the up-down ratio) and the R^2 value. Using our sample size of 30 stocks and their predictions, 
we determined that the higher the volatilty the less effective the model. 
As can be seen by the graph here.


## Intelligent Decision
However, just knowing the price data prediction is not enough to generate a sufficient profit. One must use these predictions with a strategic agent that knows how to best use them, preferably 
using reinforcement learning.  

## Ethical Consideration
This model does not break any ethical boundaries as it simply uses objective price data to find patterns within it to forecast it. As the stock market is a free market and noone knows what happens 
tomorrow, it is all fair and ethical to all competitors regardless of what methods they choose to analyze publically available data.


# Roles:

Hrishikesh:
Practical Motivation
Sample Collection
Problem Formulation
Data Preparation
Video Editing

Saanvi:
Statistical Description
Exploratory Analysis
Pattern Recognition
Analytic Visualization

Nithilan:
Machine Learning
Algorithmic Optimization
Statistical Inference and Information Presentation
Intelligent Decision
Ethical Consideration
