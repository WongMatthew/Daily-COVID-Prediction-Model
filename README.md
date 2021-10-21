# Daily-COVID-Prediction-Model

### 1st place winner of the DCP Challenge by Kings Distributed Systems at Hack The Valley 5 
HackTheValley x Distributed Compute Labs Challenge

You can find the full challenge details [here](https://docs.google.com/document/d/1xnxjDXiLMwMNr8MpUSDpuIi99Eq1Fh0tETSozf-KROY/edit?usp=sharing)

# How to use it?
Users can use the function built in the prediction model. 
```
model_prediction(data_csv, number_of_days)
```

**data_csv**: location of a csv file

**number_of_days**: number of days desired to be predicted

Users need to download their own COVID count database and input the file location with the number of days they want predicted. Function will return a csv file, prediction.csv, and the mean-squared-eror.

# How does it work? FULL BREAK DOWN
I'm provided with 3 csv files which can be downloaded from the challenge document or can be found in the repo [here](https://github.com/WongMatthew/Daily-COVID-Prediction-Model/tree/main/DCP%20Data). The csv files contain the following columns: infected_unvaccinated, infected_vaccinated and total_vaccinated. I manually added total_pop because it is given in the full challenge details. It is important to note that each csv file corresponds to a different population size. I begin with feature engineering and data exploration. I created multiple new features - daily infected, total infected and date. I graphed the new features to see how they all corresponded to each other. 

#

#### Observation 1: Date vs Daily Infected
![obs1_date vs daily infected](https://user-images.githubusercontent.com/49925170/137604782-4f73d77d-974d-44eb-a01e-420e639b06a2.png)
#### Observation 1: Date vs Total Infected
![obs1_date vs total infected](https://user-images.githubusercontent.com/49925170/137604812-f64268c4-99de-429c-8e33-7d52b2a3b77b.png)

#

After data exploration and feature engineering, I chose to use fbprophet to predict the number of those daily infected since this is a time series problem. I chose to use the date and daily infected as my ds and y variables respectively. I knew that the relation between ds and y was stationary and seasonal from my data exploration, so I set a seasonality modifier, high fourier order and a seasonality mode. Afterwards, I then fit my model and predicted for 100 days. I would then add the yhat and y to a new dataframe and used sklearn's mean-squared-error to get the MSE. I removed the excess rows and saved the predicted model. I did this for all 3 csv files. 

#

#### Observation 3 Forecast
![obs3_forecast](https://user-images.githubusercontent.com/49925170/137604821-a987b523-ea8c-4dcd-9391-84e54399cbba.png)

#

The challenge requests that I make a function that takes in two parameters, data_csv and number_of_days. Data_csv is the location of a csv file and number_of_days is the number of days to predict. The function saves a dataframe containing the predicted case count and returns the mean-squared-error and the predicted case count.  

# How does it work? TLDR
I use fbprophet to make a prediction based on the previous 300 days of infection and vaccination data for 3 different csv files for varying population sizes. At the end there's a function that saves a dataframe containing the predicted case count and returns the mean-squared-error and the predicted case count. 

# Additions Log
10/19/2021: Added Base Cases (base model with no hyperparameters) and modified the algorithm slightly for seasonality

# Whats next?
Figure out how to add a floor to prevent predictions from being less than zero, try out an ARIMA + ARMA model, normalize everything to prevent overfitting in predictions #2 and #3
