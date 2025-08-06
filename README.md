<img width="200" height="200" alt="439985615-8f615d7f-ee65-4a00-a34e-f376c185c484" src="https://github.com/user-attachments/assets/9f28d314-ed58-4885-b371-14dd59a6cede" />


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
<img width="984" height="484" alt="download" src="https://github.com/user-attachments/assets/9328f3ab-e1cd-4335-a4f1-1633063890b7" />

Many features seem to have a somewhat proportional distribution of variables with regards to the ratings, meaning there are a lot of features with what is probably no real corelation to the rating.
<img width="984" height="484" alt="download" src="https://github.com/user-attachments/assets/a335a9f0-0c10-424f-b3db-991c532d4479" />
<img width="984" height="484" alt="download" src="https://github.com/user-attachments/assets/f6ebd88d-78c7-4dd1-8f61-9a457dc22a06" />
Log transforming the data though shows that popularity and similarly the number of reviews tend to have different peaks. Popularity seems to show a slight trend of higher scores given toward the more popular trails, which makes sense. This was also noticed somewhat with the probability distribution of the number of reviews for the trail, reinforcing this trend.
<img width="984" height="484" alt="download" src="https://github.com/user-attachments/assets/91e801b2-fe5b-48f4-b106-819e5ff92027" />
<img width="984" height="484" alt="download" src="https://github.com/user-attachments/assets/24d56366-6f17-4458-a075-c46ff7fcf0d0" />
Other features like length show there's no true correlation, as the number of bad scores seems to be fairly consistent regardless of the length. 

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

In addition, learning how to take features whose data was hundreds of different lists of options and performing binary encoding on the listed objects was a new to me process that took a bit of learning to figure out how to approach. Both the features of the trails as well as the activities you could participate in on the trail were both stored as a list of almost entirely different combinations of dozens of different options per trail, so breaking them into different colums without duplicating the repeated but out of order features such as "waterfall" or "strollers" I assumed might have a great influence in a trails score, but if not encoded properly could just end up giving the model bad data to train on.

Lastly, the skew to the dataset made training difficult, as even with a R2 of .7252 the fact that the dataset didn't have many values less than 3 made it unable to learn when to predict a rating of 1-3. Of course, if nothing tends to get ratings that low, it'd make sense that a model wouldn't need to predict values that low either, however it is a flaw of the model nonetheless.

### Performance Comparison

For this the R2 value was the main metric to determine overall reliability, though RMSE was also recorded as a separate metric.

Random Forest Baseline
R2 .7037
RMSE .4406

Random Forest (tuned)
R2 .7166
RMSE .4309

XGBoost baseline
R2 .7037
RMSE .4406

XGBoost (tuned)
**R2 .7247**
RMSE .4247

Actual vs. predicted scatter plots with R² annotations.
<img width="790" height="590" alt="download" src="https://github.com/user-attachments/assets/e4b36c8c-9c4c-4130-b221-6a3d957dc563" />


Residual plots to assess variance and bias in model predictions.
<img width="790" height="590" alt="download" src="https://github.com/user-attachments/assets/c41dc4dd-fb8f-4f50-9b8f-a005ded1319b" />


### Conclusions

While the baseline looked like Random Forest would be our best model, XGBoost after parameter tuning managed to out perform it. However it's weaknesses were still obvious. Having such skewed data led to a model with a decent R2 value, however that was purely a testament to the data's skew and not the models performance as in the end the model was only able to guess high ratings, and since most ratings were high it seemed to show decent results when really a model could predict almost entirely a rating of 4 and get similar results.



### Future Work

In the future, routes to improve performance could include adding image-based features (e.g., views from trail photos) to see what kind of views impact a trails rating. 

Due to the age of this dataset as well, it would be interesting to see how these trails ratings and features have changed, as the dataset was gathered just before COVID times, and a lot of new attention has slowly been directed toward outdoor recreation since then.

Getting new data overall would be a good approach too, as with the 6+ years since the data was scraped, thousands of new reviews and trails too have been created. Obtaining more sample data in the average rating of 1-3 especially would prove extremely useful. A useful approach would be to look more at specific trails and analyze the reviews individually, determining if there are seasons that tend to get better or worse reviews, and put that against other trails at the same parks to find both similar and disagreeing trends.

## How to reproduce results

Clone this repository.

Install required packages (see below).

Run FinalModel.py.

Output plots will be saved in Data/.

### Overview of files in repository



### Software Dependencies:
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
