# Airbnb-price-prediction
Predict Airbnb properties' price using self-created convenience-score features

** Feel free to check my **[Medium Post](https://medium.com/@JennyChen_/can-propertys-convenience-score-improve-airbnb-price-prediction-performance-25e93b3aae)** for more background information. 

### 1. Installation 
- "requirements.txt"

### 2. Project Intro
**Research Question**
- In this price prediction project, the main dataset I use is from [Kaggle Seattle AirBNB Data](https://www.kaggle.com/airbnb/seattle). Except for given features provided by the datasets such as room type, property type, and the number of bedrooms, **I am interested in whether the property's closeness to popular tourists attractions or parks can affect the property's price. In other words, I am curious if adding those features can achieve better price prediction performance.**

**Motivation**
- Airbnb uses “neighborhood_group_cleansed” variable as a location feature for price prediction. That is, they assume this neighborhood indicator can explain unobserved neighborhood effects on the property’s price. However, two properties that are very close to each other but belong to different neighborhood_group_cleansed might have a similar price. Another example is that two properties that belong to the same neighborhood_group_cleansed might have huge differences in price because one is close to popular tourist attractions and the other is not, given all other amenities are the same. Therefore, I think the neighborhood_group_cleansed might not be enough to well explain the property’s price. There is some potential to investigate distance-related (or convenience-related) features and see if they help for price prediction performance.
  
**Features**
- Self-created Convenience-score-related features: 

  *1. How to identify popular POIs (point of interest) in Seattle?*
    - [Parks and recreation parks](https://www.kaggle.com/city-of-seattle/seattle-parks-and-recreation-data?select=seattle-parks-and-recreation-park-addresses.csv): it contains lat and long information of 412 parks in Seattle.
    - [Tourist Attractions](https://www.thecrazytourist.com/25-best-things-seattle-washington/). 55 Best Things to Do in Seattle (Washington), this data does not include lat and long information so I use **web-scrapping** to get location information.

  *2. Define the Airbnb property's convenience score:*
  - Given a radius threshold c (eg, c = 2km), for each Airbnb property, I create two convenience score-related variables: 
    - 1. **"cs_2_counts"**: count the number of parks (or attractions) within the c=2km radius, which sets the property as the center point. 
    - 2. **"cs_2_avgdist"**: average distance of those chosen parks (or attractions) within the c=2km radius.
  - Then, replicate the above process with various values of radius threshold. Here, I will use c=1,2,..10.

- Baseline feature set: 
  - Because this Settle Airbnb dataset has been investigated by many people on Kaggle, I will use selected features by [Zacks Shen](https://www.kaggle.com/zacksshen/kaggle-seattle-airbnb/data) as my baseline feature set.  

**Machine Learning Models**
- LinearRegression
- RandomForests

**Key Findings**
- Both Linear Regression and Random Forests suggest to include four convenience_score-related features ("cs_attr_5_counts", "cs_park_5_counts", "cs_attr_10_avgdist", "cs_park_10_avgdist") into the price prediction model for the Settle datasets, which reduce the average RMSE compared to the one without them.
- Particularly, these four features have more adds-on when we encode categorical variables using the simple 'one-hot-encoding' technique.
- It is not surprising that RandomForest outperforms the simple linear regression model.
- If we choose the RandomForests model as our final model, target-encoding for categorical variables outperforms the one-hot encoding.

### 3.File Descriptions 
- `code/convenience_score.ipynb`: generate self-created convenience-score-related features
- `code/main_AirbnbPrice.ipynb`: ETL pipeline and ML modelings

### 4. Acknowledgements 
- [Kaggle Seattle AirBNB Data](https://www.kaggle.com/airbnb/seattle)
- [Parks and recreation parks](https://www.kaggle.com/city-of-seattle/seattle-parks-and-recreation-data?select=seattle-parks-and-recreation-park-addresses.csv)
- [Tourist Attractions](https://www.thecrazytourist.com/25-best-things-seattle-washington/)
- [Kaggle-seattle-airbnb by Zacks Shen](https://www.kaggle.com/zacksshen/kaggle-seattle-airbnb/data)
