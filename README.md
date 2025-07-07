![UTA-DataScience-Logo](https://github.com/user-attachments/assets/709c1454-59e8-4387-84cb-b80ac950dfca)

# AllTrails Average Rating Estimator

* This project builds a machine learning model to predict average trail ratings based on features such as length, elevation gain, activities, and user interaction metrics. The model is trained and tuned using a real-world AllTrails dataset.

## Overview

* The goal of this project was to predict the average user rating (avg_rating) of hiking trails using a dataset compiled from AllTrails. The task was formulated as a regression problem, aiming to uncover what features most strongly contribute to higher trail satisfaction and help automate rating prediction for new or unrated trails.

Our approach involved heavy preprocessing, feature engineering from both categorical and list-type columns, and the use of ensemble models. After comparing Random Forest and XGBoost regressors, we used randomized hyperparameter search to tune our final model. The best model achieved an R² score of approximately .7252, improving on the original baseline models performance of .7052


Data

Source: AllTrails scraped dataset containing over 3,000 trails.
Format: CSV file with columns such as name, city, state, features, activities, elevation gain, difficulty, popularity, and review count.
Size: ~3,300 entries; no predefined test/validation split.
Split: 80% training / 20% testing.

Preprocessing & Feature Engineering

Dropped non-informative or high-cardinality columns (e.g., trail_id, name, visitor_usage).
One-hot encoded categorical variables: state_name, route_type.
Expanded list-type columns features and activities using MultiLabelBinarizer.
Applied log transformations to length, elevation_gain, popularity, and num_reviews to reduce skew.

#### Data Visualization

Overall distribution of target variable
![download](https://github.com/user-attachments/assets/c1e23b35-7676-42ca-88a3-502395e61516)
Heavy skew toward 4-5 as rating, I anticipate this will make getting accurate results very difficult.

![download](https://github.com/user-attachments/assets/3e46188f-3d76-4e68-8d2c-1271c03f6fb1)
![download](https://github.com/user-attachments/assets/2d347fc6-b7e2-4323-b221-7aec2bc8d291)

![download](https://github.com/user-attachments/assets/45835a25-9c1c-4712-ba2e-ed82db83e1e2)


![download](https://github.com/user-attachments/assets/a25135b2-bcf2-42d6-a84b-64f319c0f3bb)
![download](https://github.com/user-attachments/assets/143e2132-8db8-4f86-a1df-c0546967c3d0)
Many features seem to have a somewhat proportional distribution of variables with regards to the ratings, meaning there are a lot of features with what is probably no real corelation to the rating.
![download](https://github.com/user-attachments/assets/e031cf24-d173-4226-a1dc-c3c11ee5acee)


### Problem Formulation

Input: Vector of trail metadata and engineered features.
Output: A continuous score representing avg_rating (float from 1.0 to 5.0).
Models used were Linear Regression, Random Forest, and XGBoost to get a good coverage of reliable approaches. Due to very poor performance by the Linear Regression model, it was immediately dropped where Random Forest and XGBoost models were fine tuned through a gridsearch method.

### Training

Draining was done using ubuntu through the Windows Subsystem for Linux (WSL). Coding was done in python on jupyter notebook.
Training was quick thanks to the modest size of the dataset, each model only taking about 2 minutese to finish training. Though developing different ways of tweaking and fine tuning these models took up many hours.
Once it got to the point where several attempts failed to produce better results and the model was only increasing by numbers in the thousandths, and for interest of time, I called the project done.

#### Difficulties

Several roadblocks were hit in undergoing this project. The methods of feature extracting I attempted proved difficult to implement, and ultimately had to be given up on. The main hurdle was obtaining an api key from AllTrails, as they no longer have their development tools available. When I was unable to get one after days I had to move on without it and use a dataset created by someone else. As such, I was unable to verify the full accuracy and reliability of the data, and it would be out of date by several years.

In addition, learning how to take features whose data was hundreds of different lists of options and performing binary encoding on the listed objects was a new to me process that took a bit of learning to figure out how to approach. 

Lastly, the skew to the dataset made training difficult, as even with a R2 of .7252 the fact that the dataset didn't have many values less than 3 made it unable to learn when to predict a rating of 1-3.

### Performance Comparison

For this the R2 value was the main metric to determine overall reliability, though RMSE was also recorded as a separate metric.

Random Forest Baseline
R2
RMSE

Random Forest (tuned)
R2
RMSE

XGBoost baseline
R2
RMSE

XGBoost (tuned)
R2
RMSE

Actual vs. predicted scatter plots with R² annotations.
![r2_scatter_plot](https://github.com/user-attachments/assets/65dcd933-e76a-49e2-b0f1-6d881b43fa29)

Residual plots to assess variance and bias in model predictions.
![residuals_plot](https://github.com/user-attachments/assets/d249ef02-6873-4b4b-a368-b74466560391)

### Conclusions

While the baseline looked like Random Forest would be our best model, XGBoost after parameter tuning managed to out perform it. 



### Future Work

In the future, routes to improve performance could include adding image-based features (e.g., views from trail photos) to see what kind of views impact a trails rating. 

Due to the age of this dataset as well, it would be interesting to see how these trails ratings and features have changed, as the dataset was gathered just before COVID times, and a lot of new attention has slowly been directed toward outdoor recreation since then.

Getting new data overall would be a good approach too, as with the 6+ years since the data was scraped, thousands of new reviews and trails too have been created. Obtaining more sample data in the average rating of 1-3 especially would prove extremely useful.

## How to reproduce results

Clone this repository.

Install required packages (see below).

Run FinalModel.py.

Output plots will be saved in Dataset/.

### Overview of files in repository



### Software Setup
Dependencies:
pandas
numpy
matplotlib
scikit-learn
seaborn
xgboost
scipy

### Data

Pre-scraped data used was obtained from https://github.com/j-ane/trail-data/blob/master/alltrails-data.csv
As the only column with missing data was a column we didn't use for the training, the only preprocessing was mostly in the form of dropping unnecessary columns.

## Citations

data used: https://github.com/j-ane/trail-data/blob/master/alltrails-data.csv
