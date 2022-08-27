# :house:  House Prices - Advanced Regression Techniques :heavy_dollar_sign:

<img width="500" height="300" style="display: block; margin-left: auto; margin-right: auto" src="https://static1.bigstockphoto.com/1/3/9/large1500/93151490.jpg">

# :blue_book:Abstract
Predicting house prices helps people who are interested in buying a house to plan their finance well. That's why I've done this project. 

At first, I recieved a dataset with 79 attributes! Using all of them to build a machine learning model would be absurd. The final model would be too heavy, slow, inefficient and subject to overfitting. Therefore, we need to clean the dataset, removing useless or unimportant variables. To do that, we need to analyze thoroughly the dataset, see if the data is balanced or not, the number of NAN present in the dataset, make and thest hypotesis, etc etc.

Furthermore, how do we choose a model? Each dataset will have a best model to be used. We need to identify if it is a regression or a classification problem, analyse the errors values, etc.

In this project, I show how I cleanned an dataset with 79 attributes and reduced to just 12 attributes to predict house prices. It is interesting to say that it was a 83% reduction! And it correlates with Pareto's Priciple (in short, it says that, in most cases, 20% of the causes are responsable for 80% of the results).

Then, I test several regression models (such as linear regression, regularized linear regression, random forest and XGBoost) and compare them to elect the one with best result to predict house prices.

# :beginner: About This Project

The main of this project is to develop a machine learning model that predicts house sale price taking into account some attributes.

The dataset is composed by 79 attributes, the first step is to select the most important characteristics to build the machine learning model.

First, I will check for outliers, elaborate and test some hypotesis and clean our dataset off with unnecessary variables.

Then, I will test some models as Simple Linear Regression, Linear Regression Regularized, Decision Tree Regressor, Random Forest Regressor and XGBoost. To avoid overfitting, I will check using cross validation.

# :book: Dataset Information

## :warning: Missing values

This dataset contains a lot of missing values, I plotted the percentage of non-null values for each attribute and ordered it descending.

![image](https://user-images.githubusercontent.com/103580606/187033437-7182231f-68c7-43ea-8b67-0a333fbe042e.png)

As we can see, PoolQC has only 0.5% non-null values, so, if only 0.5% of the data has this information, it will not add anything to the machine learning model, so, the best way to treat this kind of missing values is to delete it. I decided to delete the columns which percentage > 30%. For the others attributes (i.e., percentage <= 30%) I decided to replace it by its mode.

* Note: Mode is the category or value that most repeat in the dataset.

## :heavy_division_sign: Splitting Numerical and Categorical attributes

It is necessary to separate the numerical attributes for those categorical. To avoid overfitting it is necessary to work in the data dimension. As the dataset has 79 different variables, using all of them will result in a complex machine learning model.

For each column, we will analyse the relative frequency of them. Those with a high frequency means that the data is very unbalanced. If the frequency is higher than 80%, I will drop the column.

Let's now analyse the percentage of each attribute. For example: 'LowQualFinSF' has 94% of its results concentrated in one single value. It means that this variable won't add significative information to explain price variation. That said, it is better if we drop it.

![image](https://user-images.githubusercontent.com/103580606/187033691-bfd2f4dd-1ce3-4290-8bde-0f519207e567.png)

The dropped numerical columns were: ['BsmtFinSF2', 'LowQualFinSF', 'BsmtHalfBath', 'KitchenAbvGr', 'EnclosedPorch', '3SsnPorch', 'ScreenPorch', 'PoolArea', 'MiscVal'].

The same was done for categorical attributes. The dropped categorical variables: 'Street', 'LandContour', 'Utilities', 'LandSlope', 'Condition1', 'Condition2', 'BldgType', 'RoofMatl', 'ExterCond', 'BsmtCond', 'BsmtFinType2', 'Heating', 'CentralAir', 'Electrical', 'Functional', 'GarageQual', 'GarageCond', 'PavedDrive', 'SaleType', 'SaleCondition'.

As we saw, we dropped the number of variables down to 47 so far. This is important to make analysis simpler and less redundant.

## :mag: Exploratory Data Analisys

This step is done to find patterns and/or anomalies. With the EDA, the first hypotesis are made.

![image](https://user-images.githubusercontent.com/103580606/187033906-020c46e2-80b0-4969-b267-770f2df584ca.png)

After that, I decided to delet the outliers values. After analyse the quartiles, the bondary values are as follow:

Upper bound = 340037.5
Lower bound = 3937.5

so, house values higher than $340000 and lower than $3937 will be dropped. 61 houses were dropped and we ended with this boxplot.

![image](https://user-images.githubusercontent.com/103580606/187034094-f763bab7-df8e-4c12-95e8-118545d8feab.png)

## :pencil: Hypotesis
Let's make some hypotesis:

### Year based:

### 1 - New houses cost more

Although the graph shows a positive slope after 1960, I don't think that we can definitely say that new houses cost more. The mean in the years close to 1890 is almost the same as 2000.

I belive that the tendecy is that, in the future, new houses will cost more, but, based in what our dataset shows, we can't say for sure. That said, I think it is not a relevant variable for us to use in our model.

### 2 - remodeled houses cost more

We concluded that this variable is not relevant for our final model!

### 3 - New houses have higher quality and cost more

This hipotesys seems to be true, it is possible to note that for houses above 1990 the grades are 7 - 10, while for years under 1990, the grades are lower. Then, I believe this is an important variable to be considered.

### 4 - Remodeled houses have higher quality and condition cost more

It is an interesting result, we can notice that both OveralQual and OveralCond has some little differences, but, don't seem to be an important variable. Thus, we can say that remodeled houses don't have different SalePrice than not remodeled.

### Quality based:

### 1 - Higher the quality, higher the price

### 2 - Higher the condition, higher the price

Both questions can be answered by the two last graphs.

Graph SalesPrice vs OverallQual shows that remodeled and non remodeled houses have almost the same prices, but, houses with higher overal quality tends to have higher sale price.

However, this relation isn't so clear for SalesPrice vs OveralCond. We can see that there is a variation, but it is not enough to prove that thare is a relation.

In conclusion, overall quality is an important variable for house prices and should be included in final model, while overal condition should not.

### Garage based:

### 1 - The type of garage has influence in the price

Houses with 2 types don't cost more than the others. The higher price comes with the builtin type and the lower is for CarPort.

### 2 - Houses without car space cost less

### 3 - houses with more capacity (number of cars) have larger garage area and cost more.

We can see that it follows the same behavir analysed in the last topic. There are a responsive difference between the amount of cars in garage, it increases until 3 garage cars, but it goes down for 4 cars. Houses with unfinished garage have lower prices and with finished garage tends to have higher price.

I belive that GarageCars is an important variable for house price, but not by itself.

### House rooms based:

### 1 - houses with more bedrooms cost more

The number of bedrooms doesn't seem to influence in the sale price of the house since we have a house with 8 bedrooms costing almost the same of a house with no bedrooms. However, the total number of rooms (excluding bedrooms) seems to have an influence over the price, the value groes up until 7 rooms and decrease for 8 and 9 rooms. Thus, I think it is an important variable for the model.

### 2 - houses with full bath + half bath cost more

Now, I am analysing the impact of 'HalfBath' and 'BsmtFullBath' in the total numer of bathrooms (i.e. 'FullBath' + 'HalfBath').

Graph 'FullBath + HalfBath per half bath' shows that it is notable that the bathroom quantity doesn't influence in price, since we have houses with 2 or 3 half bathrooms that cost less than houses without it. Then, 'HalfBath' is not an important variable.

Graph 'FullBath + HalfBath per BsmtFullBath' shows that basement bathroom seems to be importat for houses with zero and one 'FullBath + HalfBath' while for houses with three or four the influence is inconclusive. Therefore 'BsmtFullBath' is not an important variable.

We conclude that only full bath applys influence in house price.

### 3 - houses with larger GrLivArea cost more

Graph 'BsmtFinType1 vs SalePrice' shows that houses with Good Living Quarters cost more, however, for the other classifications, it seems to have no effect at all, since median is almost the same value for each classification.

Graph 'BsmtQual vs SalePrice' shows that houses with Excellent basement cost more, Typical and Fair cost almost the same.

I am not sure about the impact of both variables, so, it will be determined later using feature selection.

### 4 - houses with better kitchen quality cost more

Houses with excelent kitchen quality have higher prices and fair condition have lower. It seems to be an important attribute to define its prices.

### Outside house based:

### 1 - the neighborhood has influence in price

Neighborhood is an important variable because we can see cleary that the price chances according to the place. So, this variable will be considered in the model.

### 2 - the lot configuration has influence in price

The larger LotArea is 335 square feet and its value is $228950.

LotFrontage seems to have no influence over SalePrice, we see that for high values of LotFrontage, the price can be either high or low, thus, this is not an important variable.

LotArea also seems to have no influence over SalePrice. It is possible to see that there is a large variation up to 15000 square feet. This variation is not so intense after this value, but the house with the larger LotArea does not have the higher price. So, this is not an important variable

### 3 - lot shape has influence in price

Irregular lot shape doesn't make houses cost less, in fact, thar is no such large differente bettwen prices for different lotshape. This variable was considered not important for the ML model.

### 4 - more than one frontage makes the price increase

Houses with two or three frontage don't have higher prices. In fact, LotConfig doesn't apply a significative influence over the price.

## :heavy_check_mark: Feature Selection Itself
This section, I will make feature selection using 'SelectKBest'. I will sort the score values in descending order just for us to observe which attributes have the higher and the lower scores. Higher the score means that more that variable explains the behavior of dependent variable.

Before going to Feature Selection Itself, we need to adjust the categorical variables. To do this, I will use the function prepare_input() that I created. First, this function uses OrdinalEncoder() to transform the categorical discrete variables into intengers. After that, to avoid that the machine learning algorithm attributes a weight, we use OneHotEncoder() and, then, the StandardScaler().

We need to separate the categorical variables into two dataframes, one for discrete, other for ordinal. There isn't much to do here, we will have to do it manually.

![image](https://user-images.githubusercontent.com/103580606/187034556-6d42fb10-a4a9-4f2f-a7d5-08f1dcd0fd20.png)

After converting the ordinal categorical values in numerical values we had:

![image](https://user-images.githubusercontent.com/103580606/187034585-dcc34899-1d6e-4671-adb2-d020860380fa.png)

Now, we converted the categorical discrete variables in numerical, using the function prepare_input() and created a dataframe with all the variables.

The final data was:

![image](https://user-images.githubusercontent.com/103580606/187034701-1ea6c2f3-ef37-4705-a4e4-36aeffec1ecf.png)

Ok, now, let's check the scores. For that, I will use the select_features() function created for me. This function uses SelectKBest and mutual_info_regression. Then, I ordered it in descending order by its score value.

![image](https://user-images.githubusercontent.com/103580606/187034776-26d889c9-75b2-40c1-813f-e3e3a84b2f6d.png)

I decided to choose the attributes which the score are higher than 0.23. And the selected attributes were ['OverallQual', 'Neighborhood', 'GrLivArea', 'YearBuilt', 'GarageArea', 'GarageCars', 'ExterQual', 'TotalBsmtSF', 'KitchenQual', 'MSSubClass', 'BsmtQual', 'GarageFinish', '1stFlrSF', 'FullBath', 'GarageYrBlt']

Let's compare the features selectted by the SelectKBest and those we talked earlier.

1 - YearBuilt has a relatively high influence and will be considered for the final model. In my analysis, I said it won't be considered for the ML model, but, since the algorithm showed a high score for it, I will consider.

2 - YearRemodAdd doesn't enter in our ML model, but, we saw that it plays no influence in price.

3 - OverallQual showed to be the most valuable attribute for define SalePrice, we had discussed that it was an important variable, but the algorithm helps us to understand the true weight of the variable.

4 - OverallCond has a very little influence in SalePrice, so, it will not be considered in the final ML.

5 - GarageType also has a very little influence in SalePrice, as discussed before.

6 - GarageArea and GarageCars, they are related with each other and both apply a large influence in SalePrice.

7 - BedroomAbvGr, tot_rooms_no_bedrooms and TotRmsAbvGrd won't be cosidered in the final ML model. I thought that tot_rooms_no_bedrooms would apply some influence, but TotRmsAbvGrd showed to have more influence in it.

8 - We had considered that only FullBath applies an influence in SalePrice, and it turned out we were right.

9 - BsmtQual showed to be important for our model, when we analysed, we weren't sure about its impact in SalePrice, now, we can see that it applies.

10 - KitchenQual is important and will be considered.

11 - Neighborhood has a large influence over SalePrice, it is the second more important variable.

12 - LotArea, LotFrontage, LotShape and LotConfig don't are important, so, it won't be considered in the final ML model.

Let's see if it is possible to cut some more variables using correlation matrix.

![image](https://user-images.githubusercontent.com/103580606/187034904-a4d1c7d5-f7cb-4c62-9c27-5bccb0b05c01.png)

GarageCars and GarageArea are really related. They correlation is 0.88, so, we can eliminate one of them.

TotalBsmtSF and 1stFIrSF also have a high correlation, about 0.72, so, we can also eliminate one of them.

GarageArea and TotalBsmtSF will be dropped due to the amount of dots in x = zero. Finally, we are ready to start our ML models. At the beginnig, we had 1460 rows and 79 attributes. Now, we have only 1399 rows and 12 attributes and we are sure they are the best variables. It is a reduction of 86% in independet variable amount.

## :computer: Machine Learning Models
Let's create our X and y datasets and separe in train data and test data. But first, I will create the seed, using the function seed(42). The  values are related with the attributes and the  values are related with the SalePrice values.

Then, I will split into train data and test data, the test size will be 25% of the hole data, and the train data will be with 75%. The results of mean absolute error, mean absolut percentage error and root squared mean squared error and the cross validation values are plotted below.

![image](https://user-images.githubusercontent.com/103580606/187035061-1f282266-894f-4fff-b38b-3c2445315b31.png)

![image](https://user-images.githubusercontent.com/103580606/187035077-0238f72b-e942-43d2-8be8-7d70471f5e30.png)

XGBoost and Random Forest Regressor (RFR) have, without a doubt, the best result. XGBoost has the lowest error for every error we had analyzed, while RFR has the second lower erros.

As the results were very colse to each others, I also tried to analyse processing time, but they were also similar. We will check others results to make a conclusion.

XGBoost has the higher score, the lowest STD and, yet, the lowest erros. So, without a doubt, XGBoost is the best model for our problem.

To analyse the performance and compare each other, I will create two dataframe, one for XGBoost and the other for RFR. I will create some columns as residuals (i.e, real value - predicted value) and a column for percentage variation (i.e, (real value - predicted value) / real value).

After that, I will calculate the sum for residuals and the mean for percentage variation. Afther that I will calculate the difference bettwen than, using absolute(RFR_results) - absolute(XGBoost_results), so, if this value is higher than zero, RFR_results are higher than XGBoost_results, if this value is lower than zero, RFR_results are lower than XGBoost_results.

![image](https://user-images.githubusercontent.com/103580606/187035145-f50d0d3d-7ff3-41f3-aa06-d928a4deea72.png)

The sum of residuals for XGBoost is way higher than for RFR, which means taht the loss for RFR is lower. The percentage variantion is a very little higher for RFR, but it irrelevant.

Let's now analyse the residuals distribution for each one.

![image](https://user-images.githubusercontent.com/103580606/187035170-74bc1de2-c630-4cbd-a136-c05d09fa9567.png)

As we can see, both have a normal distribution for the residuals, however, RFR has a straighter distribution, i.e., the standard deviation is lower and their values are more concentrated near zero, so, in general, the residuals values for RFR are lower than for XGBoost.

For all the reasons presented here, I think that our XGBoost Regressor is the one with the best results, and it will be the chosen model.

Now, let's plot the graphs real vs predicted and compare the values of our model with the real values. Next, I will plot a graph for the difference between real and predicted.

![image](https://user-images.githubusercontent.com/103580606/187035184-3c1f1e95-bcd8-43e0-93d6-0102e558d1a6.png)

Our model fits very well with the test data, and that is a very good result!

## :gem: Conclusion
In this project we wanted to create a model to predict the house prices based in 79 attributes. The idea was to test some types and see the difference between then. Using the 79 attributes is absurd because it will make the model too complex, slow and very susceptible for overfitting. So, we needed to analyse what attributes were important and what were not.

Using exploratory data analysis it was possible to see the presence of outliers, and it was possible to remove them.

We had to create some hyposesis and analysed each one of then, because maybe a new variable could be created and could also be important. Afther the hypotesis tests, we used SelectkBest to see the impact of the attributes in the SalePrice. We selected those with score higher than 0.22, this is an important step, because low score means that that variable doesn't contribute anything for SalePrice chance, therefore, must be deleted.

After that, it is good to use correçation matrix to see how each variable are related to each other. That way, we were able to delete more two variable!

At the end we had 13 variables, a decrease of 83% in the number of attributes.

Finally, we created and tested several models, and, after lots of analysis, we decidade that Random Forest Regressor was the best model for our dataset.

It was my first DS project and I am really proud of each. I could learn and see that machine learning is complex, and it is not just go out creating models and 'playing with data'. It is way more complex than that, and, now, I am more confident and more able to make a good and decent analysis.


