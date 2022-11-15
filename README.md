# AlgoTrading_Bot
![](https://st2.depositphotos.com/2605379/45742/i/1600/depositphotos_457421296-stock-photo-algorithmic-trading-concept-virtual-screen.jpg)
## Summary
The Jupyter notebook included performs the following:

* Implements a simple algorithmic trading strategy that uses machine learning to automate trade decisions.
* Adjusts the input parameters to optimize the trading algorithm.
* Trains a new machine learning model (Logistic Regression Classifier) 
* Compares the models' performance to that of a baseline stratefy model.

## Results
### Baseline Strategy
The CSV file provided in the Resources folder contains 15-minute interval open, high, low, close, and volume (OHLCV) data for a Morgan Stanley Capital International (MSCI)–based emerging markets exchange-traded fund (ETF) that iShares issued. This dataset is used to develop a trading strategy and to train and test the trading algorithms.

As shown in the top figure below, the trading strategy established consists of simple-moving-average crossovers (short: period 4; long: period: 100). The figure at the bottom shows the corresponding trade signals: 1 to buy stock long; -1 to sell a stock short. Note that significant whipsaw occurs due to the lack of smoothing of the short SMA, triggering signals over short periods of time. 

![](Figures/bokeh_plot%20(8).png)
![](Figures/bokeh_plot%20(9).png)

The cumulative returns of the SMA crossover strategy established is compared with that of the underlying ETF (labeled as Actual) in the figure below. As shown, the consistant triggering of signals (whipsaw effects) result in return losses; however, these losses are recovered in 2020 over a stretch without whipsaw, resulting in a higher ultimate cumulative retun than the ETF.

![](Figures/bokeh_plot%20(10).png)

### AlgoTrading Bot 1: SVC Model
The `SVC` classifier model from SKLearn's support vector machine (SVM) learning method was used for the first algorithmic trading model. The first 3 months of the ETF close data was used to train the model, and the remaning amount of data was used for testing of the model's predictions.

The SVC classifier model yielded an accuracy of 0.67 when comparing its signal results with the actual strategy signals. As shown in the top figure below, the model misses many of the whipsaw triggers observed in the baseline strategy. This results in higher cumulative returns from the SVC model than the baseline strategy and the actual ETF, as shown in the bottom figure below.

![](Figures/bokeh_plot%20(12).png)
![](Figures/bokeh_plot%20(11).png)

### Tuning the SVC trading algorithm by adjusting the size of the training dataset
Different amounts from the ETF close price dataset were used to train the SVC model to increase its accuracy, and potentially its cumulative returns. It was observed that by using 4 years worth of data, the model performed better at predicting the test signals (accuracy:0.98) than when the 3-month long training dataset was used. Additionally, as shown in the figures below, the cumulative returns estimated from this tuned model are higher than the actual returns for the test period.

![](Figures/bokeh_plot%20(20).png)
![](Figures/bokeh_plot%20(19).png)

### Tuning the SVC trading algorithm by adjusting the SMA input features
The moving average windows of the trading strategy were adjusted to re-train the SVC algorithm. Increasing the long window produced a lag in the triggering signals, which resulted in the continuation of a long position when it should be shorted due to the price decreasing. This in turn resulted in a decrease of cumulative returns. A similar effect was observed when increasing the short window. 

A sensitivity analysis of the strategy was performed, and a short window of 10 and a long one of 60 yielded the best results due to price volatility (shown below).

![](Figures/bokeh_plot%20(14).png)
![](Figures/bokeh_plot%20(13).png)

This strategy was ultimately applied to train the SVC model, along with the 4-year worth of data. Testing of the model showed high an increased accuracy of 0.99. As shown below, the tuned model's strategy exhibits higher cumulative returns than the actual price data, plausibly as a result of the reduced triggering of signals. 

![](Figures/bokeh_plot%20(16).png)
![](Figures/bokeh_plot%20(15).png)

## New Machine Learning Classifier: Logistic Regression Classifier
A second machine learning model relying on sklearn's `LogisticRegression` was developed, employing the original parameters that the SVC model was based on (i.e., 4 to 100 SMA, and 3 months worth of training data). This model exhibited an accuracy of 0.93.

As shown below, the Logistic Regression Classifier performed better by yielding higher  cumulative returns (approx. 2X higher) than the ETF's (Actual), the original SVC model (Strategy_SVC), and baseline strategy (MA100 MA4 Strategy). 

![](Figures/bokeh_plot%20(24).png)
![](Figures/bokeh_plot%20(23).png)


When compared with the tuned SVC model considering the 4-month training data, the accuracy obtained from the Logistic Regression Classifier is the same (0.99). However, the cumulative returns calculated for the Logistic Regression Classifier are slightly higher than those of the tuned SVC model  for the same test period, as shown below.

![](Figures/bokeh_plot%20(26).png)
![](Figures/bokeh_plot%20(25).png)