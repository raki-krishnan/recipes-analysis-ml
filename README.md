# **From the Contributors to the Calories: An Exploration of Recipes - by Raki "Gordon Ramsay" Krishnan**

Contact: raki@umich.edu

## **Introduction**

I am a hungry person, both for knowledge and food (😆). I am also a big fan of Gordon Ramsay (👨‍🍳😡). Consequently, I decided that the most interesting data to analyze would be recipes for delicious dishes. The recipes I analyzed are pulled directly from [food.com](https://www.food.com). Each row in my dataframe contains a recipe, and each column contains useful information about the recipes. Displayed below is information about my dataframe, including the shape and column names.

### Shape of the Recipes Dataframe: 

So what was I working with once I cleaned the data and was left with my dataframe ready for analysis?

```py
#find out the number of rows and columns in the dataframe
print(recipes.shape)
```
(234429, 24)

The above result from the code means that there are 234,429 rows and 24 columns in my recipes dataframe!

### Exploratory data analysis (EDA) Question: 

<span style="color: limegreen; font-weight: bold;">Which contributors consistently have the highest average recipe ratings?</span>

This is an important question to think about, because if I am ever super hungry in the future (and also actually learn how to  cook), I will know which contributors' recipes to look at. Perhaps I can also learn to cook by following these top contributors. After performing this EDA, we will know which contributors are the most esteemed on food.com.

### Which columns were most important to my EDA?

The columns that are most related to my EDA are 'contributor_id', 'rating', and 'average_rating'.

<span style="color: limegreen; font-weight: bold;">contributer_id</span>: A unique way to identify the contributors (also known as primary key). This column is of type integer. \
<span style="color: limegreen; font-weight: bold;">'rating'</span>: A single rating of the recipe given by a user. This column is of type integer. \
<span style="color: limegreen; font-weight: bold;">'average_rating'</span>: The mean rating that a recipe was given. This column is of type float.

## **Data Cleaning and Exploratory Data Analysis**

Cleaning the data involved reading the csv files, creating tables, merging the tables, and formatting the data so that it was easy to work with. The next three lines of code display how I attained the data from the csv files and merged them into one dataframe:

```py
#Read the dataframes from their csv files and left merge them
recipes_df = pd.read_csv('food_data/RAW_recipes.csv')
interactions_df = pd.read_csv('food_data/RAW_interactions.csv')
merged_df = pd.merge(recipes_df, interactions_df, how='left', left_on='id', right_on='recipe_id')
```

A left merge indicates that all the records from the "left" dataframe, in this case recipes_df, will be kept, while only matching records from the "right" dataframe, in this case interations_df, will be kept. Once I had the merged dataframe, I had to make sure the data was easy to work with. Given that I was exploring a question relating to ratings of recipes, I checked out the distribution of ratings and visualized it (shown below).

<iframe
  src="assets/dist_of_ratings_before_replacing.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>
As we can see, there are over 15,000 ratings of 0 stars. This is problematic because the rating scale is 1-5 stars, not inclusive of 0, so these ratings are probably from people who just left the star rating blank when writing their review. To be able to answer my question more accurately, I replaced all ratings of 0 with np.nan. The new distribution of ratings is shown below.

<iframe
  src="assets/dist_of_ratings_after_replacing.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>

One more thing I did was I created an average_rating column. Originally, the data only had individual ratings for each review, so I thought it would be meaningful to group the dataframe by recipe_id, calculate the average rating for each recipe, and add this as a column. 

The last step I took in cleaning the data was organizing the 'nutrition' column. The nutrition column contained information in the form of a string that looked like a list: "[calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates]". I took this information and created separate columns called 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbohydrates'. The head of my cleaned and tidy dataframe is shown below.

| name                              |     id |   minutes |   contributor_id | submitted   | tags                              |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |   average_rating |
|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|
| 1 brownies in the world    bes... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |                4 |
| 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |

### Univariate Analysis

So I had a clean dataframe, but what's next? I was looking to answer the question "**Which contributors consistently have the highest average recipe ratings?**" A good way to start analyzing this is to create a column called 'num_contributions' and display its distribution. I created the column 'num_contributions' by grouping the dataframe by 'contributor_id' and counting how many times that contributor appeared. Given the sheer amount of contributors that contributed between 1-99 times, I decided to drop these datapoints when showing the distribution of 'num_contributions'. This made the data much less skewed and also more meaningful to look at, as my EDA question involved looking at contributors that **consistently** have the best ratings. Below is a boxplot showing the distribution of 'num_contributions' for contributors who contributed more than 100 times.

<iframe
  src="assets/contributions_per_contributor.html"
  width="850"
  height="1100"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -300px;"
></iframe>

As we can see, the median number of contributions for contributors that contributed over 100 times is 247. Interestingly, one contributor contributed 3,060 times (the maximum). That is one dedicated person!

Another interesting column to see the distribution of was the 'average_rating' column. This column showed the average rating of recipes. However, to make the distribution meaningul, I had to first adjust the dataframe so that the recipe_id column only contained a recipe once. Since this column is an aggregate, there was no sense in displaying the average rating of a single recipe multiple times. Below shows how I did this in code:

```py
#Remove duplicate rows with respect to recipe_id
unique_recipe_df = recipes.copy()
unique_recipe_df = unique_recipe_df.drop_duplicates('recipe_id')
```

Below is the distribution is shown in a histogram.

<iframe
  src="assets/average_rating_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As we can see, food.com reviewers seem to be quite supportive on average. Most recipes got an average rating of 4 or above! Over 48,000 recipes got an average rating between 4.9 stars and 5 stars. Another interesting pattern is that the average ratings tend to be closer to integers (i.e. 1 star, 2 stars, 3 stars, etc) rather than decimals (i.e. 1.3 stars, 2.6 stars, 3.8 stars, etc).

### Bivariate Analysis

After displaying the distribution of single columns, I looked at the distribution of multiple columns. Specifically, I answered my question with a cool interactive plot that looked at the top ten contributors by average rating. I configured the bar plot so that the user can change the minimum threshold for number of contributions for the contributor to be displayed. The reason I started the minimum threshold at 100 is that there are an overwhelming number of contributors with a small amount of contributions and an average rating between 4.9-5 stars. Additionally, I wanted to explore the contributors who were most **consistent**. The number inside of the bar is the number of contributions a contributor made. This plot is shown below.

<iframe
  src="assets/top_10_contributors_by_avg_rating.html"
  width="1000"
  height="700"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -270px;"

></iframe>

One trend to notice is that as you increase the minimum contribution threshold, the top ten contributors' average recipe ratings decrease. This makes sense because it is extremely difficult to maintain 5 star ratings when you have made more recipes. However, the top ten contributors' average recipe ratings always stays above 4.5 stars. Also, we can connect back to seeing a contributor that contributed <span style="color: limegreen;">3,060 recipes</span> (from our box plot of the num_contributions distribution.). That person was contributor 37449 and has an average rating of <span style="color: limegreen;">4.787 stars</span>. That is super impressive!

### Interesting Aggregate Table

Below is a table showing the aggregates of (1) average count of contributions each contributor made and (2) the average rating for those contributors. This table helps us visualize the trends of the above graph in tabular format. For this table, I made the threshold of minimum contributions 488.5 because that is the third quartile of number of contributions, for contributors with 100+ contributions, as shown in our boxplot (Contributions per Contributor). In my original EDA question, we could define the term "consistent" however we want. In this case, I am defining "consistent" as a contributor who's number of contributions is above the third quartile in the IQR. This table is significant because it also helps us see that, given my definition of a consistent contributor, <span style="color: limegreen;"> contributor 461834 is our top contributor, as they contributed 1680 recipes and had an average rating of 4.85 stars </span>  

|   contributor_id |   num_contributions |   avg_rating |
|-----------------:|--------------------:|-------------:|
| 461834           |                1680 |      4.85124 |
| 128473           |                1091 |      4.84722 |
| 226863           |                2754 |      4.84693 |
|      1.68072e+06 |                 492 |      4.84634 |
| 526666           |                 872 |      4.84148 |
| 227978           |                 688 |      4.83164 |
| 482376           |                 969 |      4.82969 |
| 174096           |                 705 |      4.82713 |
| 266635           |                 994 |      4.82578 |
| 169430           |                2310 |      4.82568 |


## **Framing a Prediction Problem**

After that interesting EDA, I decided to shift gears. I decided to predict the 'calories' column of my recipes dataframe. This is a supervised learning regression problem because calories is a quantitative variable. I am getting my wisdom teeth taken out soon, and am going to need to eat a lot of soup in the following days. To make it more interesting, I decided to specifically <span style="color: limegreen;">predict the 'calories' column for soup recipes</span>. 

I determined which recipes were soup recipes by filtering my dataframe based on the name of the recipe including the substring "soup". Below is the line of code showing how I did it:

```py
#Keep only rows where the name of the recipe contains 'soup'
recipes_copy = recipes.copy()
recipes_copy = recipes_copy.loc[recipes_copy['name'].str.contains('soup', case=False, na=False)]
```
Note: The case=False parameter makes it so a case-insensitive search will occur

Now that I have created this smaller dataframe called recipes_copy, it is important to know how many rows are in the dataframe. After calling recipes_copy.shape, I saw that there are 9628 rows.

Before moving forward, it is important to understand what features we may know at time of prediction. In this case, we would know all of the nutritional variables (i.e. protein, carbs, fat, etc) as they are always displayed with the recipe. We will also always know the ingredients being used and the number of steps, as those are crucial parts of recipes. 

## **Baseline Model**

My baseline model uses two features to predict the 'calories' column. Those two features are 'carbohydrates' and 'sugar', both quantitative features. As a result, I did not have to perform any one-hot encoding. Before training and testing my model, one thing I checked was whether there were any NaN values in the calories column, as calories is the target variable. 

```py
print("Number of NaN calories columns:", recipes_copy['calories'].isna().sum())
```

Running this code printed out 0. It appears that the contributors that make soup are good about inputting the number of calories in their recipes.

After creating my train test split, I created a pipline using the LinearRegression() model, as shown below.

```py
#Create pipeline using StandardScaler() and LinearRegression()
pipeline = Pipeline([
    ('scaler', StandardScaler()),
    ('regressor', LinearRegression())
])
```


Below is a 3D graph showing the predicted and actual values of my baseline model. Feel free to drag your mouse and check it out from different angles!

<iframe
  src="assets/3D-plot.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>

This graph is a 3-dimensional scatterplot that shows the distribution of actual and predicted values for the calories column. It appears that the model did a decent job, as there is a lot of overlap between the red and blue. Additionally, I used the mean_squared_error function from sklearn.metrics to calculate the mean-squared error (MSE) of this model. It has a MSE of  <span style="color: limegreen;"> 26388.78</span>. 

Given that there are 9628 rows in the dataframe this MSE not bad. However, the MSE does not tell the full story. I do not think that this model is very good overall. Firstly, the model relies only on 'carbohydrates' and 'sugar' to predict 'calories'. While these are important factors to indicate calorie content of soups, there are many more features that could be added to improve the predictions. Additionally, while standard scaling helps in handling features with different units or scales, it doesn't address the potential issue of outliers or non-normal distributions of features, which can negatively affect model performance. 

## **Final Model**

My final model is a Ridge regression model with λ=1.0 and features 'sugar', 'protein', 'sodium', 'carbohydrates', and 'n_steps'. However, determining these features, determining that Ridge Regression was the best type of regression to use, and finding the hyperparameter was a long road. Let's discuss how I got to this point. 

### Features

Below is a list of the columns in the recipes dataframe, along with their datatypes as given by recipes.dtypes

```
name               object
id                  int64
minutes             int64
contributor_id      int64
submitted          object
tags               object
n_steps             int64
steps              object
description        object
ingredients        object
n_ingredients       int64
user_id           float64
recipe_id         float64
date               object
rating            float64
review             object
calories          float64
total_fat         float64
sugar             float64
sodium            float64
protein           float64
saturated_fat     float64
carbohydrates     float64
average_rating    float64
``````

All of the columns of datatype "object" are columns containing strings, whether formatted in a list of strings or just a singular string. None of these columns made sense to incorporate into my model when predicting calories, as there is no significant relation between any of these columns and the number of calories. 

That left me with the quantitative columns to choose from. To do this, I took a look at the correlation between the potentially relavent quanititative categories and the calories column, using the code snippet below:

```py
print("\nDisplaying correlation between variables and calories:")
print(recipes_cleaned[numeric_categories].corr()['calories'])
```

Here are the results:
```
Displaying correlation between variables and calories:
n_ingredients     0.182
minutes           0.001
calories          1.000
carbohydrates     0.794
sugar             0.475
protein           0.843
sodium            0.582
total_fat         0.771
average_rating   -0.022
n_steps           0.113
```

The most correlated features to calories are carbohydrates, protein, total_fat, and saturated fat. There is clearly multicollinearity of total fat and saturated fat, since saturated fat is a subset of total fat. This means that they are essentially (perhaps not perfect) linear combinations of each other. And through my testing of the model, I saw that  when carbohydrates and total fat were both included features on the model, this caused overfitting. Therefore, out of 'carbohydrates', 'total_fat', and "saturated_fat', it only made sense to choose one of these features for my final model. Since carbohydrates has the highest correlation to calories, while not having such a high correlation that it would dominate all other features, I chose <span style="color: limegreen;">'carbohydrates'</span> to be my first feature.

Next, pairing the 'carbohydrates' feature with the 'protein' feature did not end up causing overfitting. The correlation coefficient between 'carbohydrates' and 'protein' is 0.626. This makes sense because many soups can have a high content of carbohydrates, while having a low content of protein (and vice-versa) in terms of percent daily value. Thus, the next feature I included in my final model was <span style="color: limegreen;">'protein'</span>.

The columns 'sugar' and 'sodium' both have moderate positive linear relationships with the 'calories' column, and thus these were good candidates. Additionally, a high sugar content in a soup does not indicate a high sodium content (and vice-versa), so including both of these would not introduce multicollinearity. The correlation coefficient between these two columns is 0.56. Therefore, the next two features I added into my model were <span style="color: limegreen;">'sugar'</span> and <span style="color: limegreen;">'sodium'</span>.

The 'average_rating' and 'minutes' columns do not appear to have any linear relationship with the 'calories' column, so I excluded these columns. Additionally, 'average_rating' may not be something we always know at time of prediction (i.e. if there are no ratings on the recipe yet). Including either of these columns would have just added unnecessary noise and potentially made the predictions less generalizable to unseen data.

The last two columns to consider adding as features were 'n_steps' and 'n_ingredients'. By finding a correlation coefficient of 0.29 between these two columns, I saw that there is a slight correlation between 'n_steps' and 'n_ingredients'. Therefore, it would not be pertinent to put both in the model, especially given that they are both only very loosely correlated to 'calories'. However, adding in one of these columns as a feature could still provide some value, as there is a loose correlation to 'calories', and the regularization from the regression model I use would account for this by putting a small coefficient on the variable. Thus, I originally decided to try adding 'n_ingredients' as a feature, because it has a slightly higher correlation to calories, but surprisingly it resulted in a negative coefficient. This is counterintuitive, and thus a questionable feature to add into my model. Therefore, I landed on using <span style="color: limegreen;">'n_steps'</span> as my final feature.

In summary, my final features list looked like this:

```py
features = ['carbohydrates','protein', 'sugar','sodium','n_steps']
```

### Hyperparameter Optimization & the Decision Between Ridge vs Lasso

Once I determined the optimal features for my model, I had to decide which type of model to use: A Ridge regression model or Lasso regression model? Before comparing the models, I had to first find their respective optimal hyperparameters. I did this through using sklearn's GridSearchCV using 5 folds for the cross-validation and negative mean squared error as scoring. This resulted in finding that 1.0 was the best hyperparameter for Ridge regression and 0.1 was the best hyperparameter for Lasso regression.

Once I had these optimal hyperparameters, I was able to run each pipeline I creating, using StandardScaler() and the respective type of regression, and see the results of the predictions. Below is displayed the MSE for each model using sklearn.metric's mean_squared_error() fucntion, and the cross validation MSEs using sklearn.model_selection's cross_val_score() function:

<div style="border: 2px solid limegreen; padding: 10px;">

Ridge Regression: Mean Squared Error = 12835.58<br>
Lasso Regression: Mean Squared Error = 12835.22<br><br>
Ridge MSE Scores: [13718.33, 16171.17, 9810.49, 14111.67, 17772.22]<br>
Lasso MSE Scores: [13712.31, 16175.31, 9806.52, 14107.02, 17776.29]<br><br>
Average MSE for Ridge Cross Validation: 14316.774243077636<br>
Average MSE for Lasso Cross Validation: 14315.490150292608

</div>

The MSEs are almost identical! This made sense once I printed out the coefficients of each model:

<div style="border: 2px solid limegreen; padding: 10px;">

Ridge Coefficients: <br>
carbohydrates: 121.5504 <br>
protein: 160.0611 <br>
sugar: 9.5887 <br>
sodium: 20.5648 <br>
n_steps: 11.8974 <br><br>
Lasso Coefficients: <br>
carbohydrates: 121.5470 <br>
protein: 160.0492 <br>
sugar: 9.5192<br>
sodium: 20.5039<br>
n_steps: 11.8050

</div>

The coefficients are also very similar!

It seems like the choice between Ridge and Lasso could be a toss-up here, as they have nearly identical performance and coefficients. In the end, I decided Ridge Regression would be more appropriate to answer this specific question. This is because because Lasso regression is more likely to completely make features obsolete, which might not generalize as well compared to Ridge regression. If we were to continue building this model with more features and data, I believe that for the aformentioned reasons, <span style="color: limegreen;"> Ridge Regression </span> would generalize better to unseen data.

### Final Model vs. Baseline Model

The most obvious improvement that my final model made compared to my baseline model is the large decrease in mean-squared error. My baseline model has a MSE of 26,388.78 while my final model has and MSE of 12,835.58. This is a difference of approximately 13,553.

However, a decrease in MSE does not alone prove that my final model was better. Another reason that the final model is better is that the baseline model only uses two features, while the final model uses five features. All five of these features have strong backing to have been included in the model while not causing overfitting. To prove this, below is a heatmap I created showing the correlation of my five features:

<iframe
  src="assets/corr_heatmap.html"
  width="1000"
  height="1000"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>

As the model shows, the highest relation between features is 'carbohydrates' and 'protein' with a correlation coefficient of 0.626. The general threshold that you want maintain to avoid multicollinearity is features with a correlation coefficient of greater than or equal to 0.8. Therefore, these features all work very well together to produce a much-improved model.

One last reason that the final model is better than the baseline model is that the final model uses Ridge Regression, which is much better equipped to handle multicollinearity, reduce overfitting, and generalize to unseen data.

# Thank you and I hope you enjoyed reading through this!!!