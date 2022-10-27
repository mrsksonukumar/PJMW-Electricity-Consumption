# Business Objective
PJM Interconnection LLC (PJM) is a regional transmission organization (RTO) in the United States. It is part of the Eastern Interconnection grid operating an electric transmission system serving all or parts of Delaware, Illinois, Indiana, Kentucky, Maryland, Michigan, New Jersey, North Carolina, Ohio, Pennsylvania, Tennessee, Virginia, West Virginia, and the District of Columbia. The hourly power consumption data comes from PJM's website and are in megawatts (MW). The regions have changed over the years so data may only appear for certain dates per region.

Throughout this project, we will be building a model to forecast monthly electricity consumption from PJM'S website. This model will be based on time series data.

# Project Architecture
<p align="center">
  <img width="350" height="350" src="https://user-images.githubusercontent.com/103531894/198220601-c0f3202b-bc3d-486a-8a9b-dfdff311f442.png">
</p>

# Exploratory Data Analysis
First, we will need to load the modules we will be using as well as our dataset. When our data was originally stored in the CSV file, timestamps were converted to strings. We need to parse them back into datetime objects for our analysis and set them as the dataset index. This can be done automatically using the parse_dates and index_col parameters for the read_csv function included with Pandas. Once we have established a datetime index, we sort the values to make sure they are presented in chronological order. Skipping this step will reveal that the timestamps may have been sorted by their timestamp strings previously, as some timestamps are out of order.

A quick evaluation of the data indicates that it has 143,206 rows and one column (PJMW_MW). We will now go through the data to identify duplicate timestamps, impute missing values and prepare some basic visualizations.

A quick search through the dataset indicates that there are 30 missing values and 4 duplicated values across our date range. We've used mean interpolation to impute these missing values and dropped the one duplicates.

# Data Understanding

With this volume of data, we can’t see much with the exception of some possible fluctuation linked to seasonality throughout the year. Let’s take a look at annual average consumption. We have observed that high amount of energy consumed in the year 2002-2006 and 2018 compared to the other years.

Also, For summer and winter months typically have higher energy demand while spring and fall are lower. This matches intuition that months with more extreme temperatures have more energy consumption (likely due to increased use of air conditioners/fans and heaters).

Energy consumption on Saturday and Sunday are lower. This reflects the intuition that energy use is typically lower on the weekends. We have observed the daily trend of energy consumption decreasing from 08:00 PM to 05:00 AM. This reflects intuition that energy demand is lower at night when consumers are typically asleep. We also see an upward trend between 05:00 AM to 07:00 PM. This reflects intuition that energy demand increases as consumers begin their day and evening.

# Error-Trend-Seasonality Decomposition

Error-Trend-Seasonality is a model used for the time series decomposition. It decomposes the series into the error, trend and seasonality component. It is a univariate forecasting model used when dealing with time-series data. It focuses on trend and seasonal components.

We have observed no trend and sesaonality in the dataset. And because of the constant trend we see the Normal Distribution.

# Stationarity Check

Stationary data is data whose statistical properties like mean, variance, covariance do not vary with time or these statistical properties are not the function of time. a stationary data series has without a Trend or Seasonal components. Stationary series is easier for statistical models to predict effectively and precisely.

We have used Augmented Dickey Fuller’s Test(ADF) to test the stationarity in the dataset. We got P-Value as zero because there is no trend in the dataset as stated earlier.

# Resampling of Dataset
The dataset have very large number of data points. So, we have down sampled our data for reducing the number of data points. By applying resample method the hourly data converted into daily data by which number of data points decreases into 143,232 to 5,969.

# Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF)

To determine the moving average and autoregressive model orders that are most appropriate for our data, we have used the Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF), respectively. To understand what these are, we will need to define some additional terms.

Autocorrelation refers to the correlation between values of a time series and lagged values of the same time series. It represents the degree of correlation between present and past values of a time series. The autocorrelation of a time series can be calculated for several different lag values and plotted. This plot is known as the Autocorrelation Function, or ACF.

The Partial Autocorrelation Function, or PACF, summarizes the relationship between present and past values within a time series but removes the effects of any potential relationships at intermediate, or “lower-order”, time steps. A typical autocorrelation includes effects from both the direct correlation between an observation at a lagged time value and the present value as well as the correlations between intermediate time values and the present value. By removing the intermediate correlations, we are left with a PACF plot that we can use to determine the order of our moving average model. 

In the dataset we did not get any considerable value of P and Q becuase the ACF and PACF relation for the resampled data was without any differentiation.

# Seasonality & Trend

Seasonality is not addressed directly using a standard ARIMA model. However, we can use an ARIMA model adapted to manage seasonality to model data with clear seasonal trends. However, as we move forward to our model building we have used Linear Regression for that matter.

# Splitting the dataset into Train and Test

We have used 80% of the dataset for training purpose and 20% for testing. Here we are training 5754 rows and one column and we are testing on 215 rows and one column.

# Model Building

Model building has been done on Stationary and Non-Stationary datset. The models we have used is listed below:

* Simple Exponential Smoothing
* Holt's Method
* Holt's - Winter method
* ARIMA
* SARIMA
* Linear Regression Model

| Sr. No.  | Forecasting Model | Model Parameter | MSE | RMSE | MAPE |
| ---------| ----------------- | --------------- | --- | ---- | ---- |
| 1. | AR(1) | (1,0,0) | 54063.14 | 232.51 | 8.26 | 
| 2. | MA(1) | (0,0,1) | 202082.40 | 449.54 | 14 |
| 3. | ARMA | (1,0,1) | 35040.54 | 187.19 | 4.66 |
| 4. | ARIMA | (1,1,1) | 14020.00 | 118.41 | 9.03 |
| 5. | SARMIA | (0,1,2) (0,1,0) | 3526020.17 | 1877.77 | 223.21 |
| 6. | FB Prophet | Auto Forecasting | 165880.68 | 407.28 | 116.66 |
| 7. | Regression Model | Random Forest | 802816.00 | 896.00 | 98.00 |
| 8. | Regression Model | xgBoost | 581346.78 | 762.46 | 123.63 |
| 9. | Regression Model | Linear Regression | 347321.63 | 589.34 | 8.54 |

We finalised Linear Regression model for forecasting and deployment as it is giving considerable accuracy and convenient for deployment.

# Deployment

You can wath the deployed App on the below link :
https://mrsksonukumar-pj-pjmw-energy-consumption-forecasting-app-rlxgzq.streamlitapp.com/

 
 <p align="center">
  <img width="350" height="350" src="https://linguaholic.com/linguablog/wp-content/uploads/2021/03/A-Huge-Thank-You-735x413.jpeg.webp">
</p>
 
 
