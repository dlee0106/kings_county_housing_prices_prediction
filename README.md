# Predicting Housing Prices in Kings County, Seattle

Authors: Donna Lee

## Overview 

The goal of this project is to find a model that best predicts housing prices in Kings County, Seattle. The model was trained on 17,000+ sales in 2014 and 2015 from the area. I examined the data to understand what variables impacts housing prices and used a regression model to predict the prices of homes sold. 

## Data Investigation & Cleaning

There are some key assumptions everyone holds about the housing market so I decided to start there. Some initial variables I looked at included: 
* *Square Foot Living* 
  - We found an initial positive correlation between the two variables
  - ![sqft_living_and_price](https://github.com/dlee0106/kings_county_housing_prices_prediction/blob/main/sqft_living_and_price.png)
* *Number of Bedrooms & Bathrooms*
  - This is related to square foot living but we expect there to be a generally positive relationship between number of bedrooms and bathrooms. There were some anamolies in there data such as a house with 33 bedrooms going for only $640,000.
  - In order to clean the bedrooms column, we know that most of the houses sit at under 8 bedrooms so anything over that I just capped at 8 bedrooms. 
* *Selling Season*
  - Here we ran an ANOVA test to determine if there was a significant difference between houses sold during each season. Our p-value was lower than our alpha so we rejected our null hypothesis that there was no difference between the different seasons. 
* *Renovated (Yes/No)*
  - After running a t-test on renovation status, we found that the average price between houses that were renovated versus not were not the same. 
  - ![Average Price by Renovation Status](https://github.com/dlee0106/kings_county_housing_prices_prediction/blob/main/avg_price_by_renovations.png)


## Initial Model
Our initial OLS model looked at the relationship between price and:
* Bathrooms
* Bedrooms
* Square foot living
* Floors
* Waterfront
* Square foot living for the nearest 15 neighbors
* Age of house
* Whether or not it was renovated
* Yard size
* Grade
* Zip Code
* View
* Month it was sold
* Condition

This initial model did an average job of predicting housing prices. Plotting the residuals (see QQ plot below), we saw that it wasn't very good at predicting the extreme values which led me to believe it wasn't a linear relationship. 

![initial_model_qqplot_resid](https://user-images.githubusercontent.com/76017120/110979825-1becb300-8333-11eb-8e03-0d2e6171b45f.png)

## Second Model
Looking at the residuals, I decided to take the log of the price to see if we can better fit the model. Looking at the new residual plot, we can see that taking the log of the target variable was a better fit. 

![second_model_qqplot_resid](https://user-images.githubusercontent.com/76017120/110980118-8140a400-8333-11eb-856d-81314f26b561.png)

Seeing the impact of taking the log of the target variable on our model, I decided to also take the log of square foot living and square foot living of the neighboring 15 properties to further fit the model. This brought our RMSE for the training set down to around **130K**.

## Feature Selection 
Since we have 113 columns, I wanted to see if we could dwindle it down to just the most important ones. I used an F-test to check for columns that I could remove. Unfortunately this model did not perform better than our original log model. We decided to go ahead and run the original log model on the holdout data. 

Stay tuned for how that performed!
