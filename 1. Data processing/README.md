All these processes implemented in Preprocessing.ipynb

# Data Preprocessing

According to modern machine learning approaches, ML projects tend to be more [data-centric rather than model-centric](https://www.youtube.com/watch?v=06-AZXmwHjo&t=1142s). To this end, we need to make more effort to this important part of a machine learning process.

## Introduction

After a heavy R&D on this problem, I categorize this task as a Sentiment Analysis task. Sentiment Analysis is all about systematically identify, extract, quantify, and study affective states and subjective information. After finding the task category it is time to start.

## Preprocessing
After I downloaded the **raw_analyst_ratings.csv** file, I go through these steps:

### Exploratory data analysis

In this step, I load the CSV file with pandas and quickly found that this dataset has no label column. Then I remove duplication to prepare the dataset for data labeling.

#### Data Labeling

I have two choices for labeling data:
* Using another model to analyze the headlines or web page contents and determine the sentiment.
* Using actual stock history to see what the impact of published news on a stock value is(raised or dropped).[**chose**]

##### Stock history data gathering

I use [yahoo finance stock API](https://github.com/ranaroussi/yfinance) to gather the price of a stock on a specific date. there is a lot of stock API out there but I decided to use yfinance because it is free and open source. I downloaded and stored stock history for a while in a dictionary with tickers as key and save it as a pickle serialized object to avoid redundant API calls in the future.
For each row in the dataset here is the labeling process:

For each row in the dataset here is the labeling process:
1. Finding the close price of the stock on that date.
2. Finding the close price of the stock on the next business day
3. Comparing the prices and if increased, assign 1 otherwise 0.
4. If there is no history for a specific stock that row labeled with NAN and that row will remove.

### Feature Selection

Given that we have a sentiment analysis problem, for sake of time I have selected the headline feature. It could be done better using the crawled content of the news using DAGs with more attention to the previous and next sentence of a sentence that contains the name of stock to provide more data for the future model.

## Exporting labeled dataset

Now we have labels for each row that indicate a stock value will be raised or not. The **increased** column is our target feature for the model. The **labeled.csv** is the final file. For performance purposes, I ran the preprocess operations on Google Colab.



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>headline</th>
      <th>url</th>
      <th>publisher</th>
      <th>date</th>
      <th>stock</th>
      <th>increased</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Stocks That Hit 52-Week Highs On Friday</td>
      <td>https://www.benzinga.com/news/20/06/16190091/s...</td>
      <td>Benzinga Insights</td>
      <td>2020-06-05 10:30:54-04:00</td>
      <td>A</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Stocks That Hit 52-Week Highs On Wednesday</td>
      <td>https://www.benzinga.com/news/20/06/16170189/s...</td>
      <td>Benzinga Insights</td>
      <td>2020-06-03 10:45:20-04:00</td>
      <td>A</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>71 Biggest Movers From Friday</td>
      <td>https://www.benzinga.com/news/20/05/16103463/7...</td>
      <td>Lisa Levin</td>
      <td>2020-05-26 04:30:07-04:00</td>
      <td>A</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>46 Stocks Moving In Friday's Mid-Day Session</td>
      <td>https://www.benzinga.com/news/20/05/16095921/4...</td>
      <td>Lisa Levin</td>
      <td>2020-05-22 12:45:06-04:00</td>
      <td>A</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>B of A Securities Maintains Neutral on Agilent...</td>
      <td>https://www.benzinga.com/news/20/05/16095304/b...</td>
      <td>Vick Meyer</td>
      <td>2020-05-22 11:38:59-04:00</td>
      <td>A</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>




    Number of rows in original dataset: 1407328
    Number of rows in labeled dataset: 1052624
    


    count of 0 values in labeled dataset: 530381
    count of 1 values in labeled dataset: 522243
    
