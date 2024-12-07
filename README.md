# **From the Contributors to the Calories: An Exploration of Recipes - by Raki "Gordon Ramsay" Krishnan**

## **Introduction**

I am a hungry person, both for knowledge and food (😆). I am also a big fan of Gordon Ramsay (👨‍🍳😡). Consequently, I decided that the most interesting data to analyze would be recipes for delicious dishes. The recipes I analyzed are pulled directly from [food.com](https://www.food.com). Each row in my dataframe contains a recipe, and each column contains useful information about the recipes. Displayed below is information about my dataframe, including the shape and column names.

### Shape of the Recipes Dataframe: 

So what was I working with once I cleaned the data and was left with my dataframe ready for analysis?

```py
print(recipes.shape)
```
(234429, 24)

The above result from the code means that there are 234,429 rows and 24 columns in my recipes dataframe!

### Exploratory data analysis (EDA) Question: 

**Which contributors consistently have the highest average recipe ratings?**

This is an important question to think about, because if I am ever super hungry in the future (and also actually learn how to  cook), I will know which contributors' recipes to look at. Perhaps I can also learn to cook by following these top contributors. After performing this EDA, we will know which contributors are the most esteemed on food.com.

### Which columns were most important to my EDA?

```py
print(recipes.columns.to_list())
```
['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients', 'user_id', 'recipe_id', 'date', 'rating', 'review', 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', 'carbohydrates', 'average_rating']

Above is a list of all the column names in the cleaned recipes dataframe that I used for analysis.

The columns that are most related to my EDA were 'contributor_id', 'rating', and 'average_rating'.

<span style="color: limegreen; font-weight: bold;">contributer_id</span>: A unique way to identify the contributors (also known as primary key). This column is of type integer. \
<span style="color: limegreen; font-weight: bold;">'rating'</span>: A single rating of the recipe given by a user. This column is of type integer. \
<span style="color: limegreen; font-weight: bold;">'average_rating'</span>: The mean rating that a recipe was given. This column is of type float.

## **Data Cleaning and Exploratory Data Analysis**

Cleaning the data involved reading the csv files, creating tables, merging the tables, and formatting the data so that it was easy to work with. The next three lines of code display how I attained the data from the csv files and merged them into one dataframe:

```py
recipes_df = pd.read_csv('food_data/RAW_recipes.csv')
interactions_df = pd.read_csv('food_data/RAW_interactions.csv')
merged_df = pd.merge(recipes_df, interactions_df, how='left', left_on='id', right_on='recipe_id')
```

Once I had the merged dataframe, I had to make sure the data was easy to work with. Given that I was exploring a question relating to ratings of recipes, I checked out the distribution of ratings and visualized it (shown below).

<iframe
  src="assets/dist_of_ratings_before_replacing.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>
As we can see, there are over 15,000 ratings of 0 stars. This is problematic because the rating scale is 1-5 stars, not inclusive of 0, so these ratings are probably from people who just left the star rating blank when writing their review. To be able to answer my question more accurately, I replaced all ratings of 0 with np.nan. In this case, I utilized the missing data imputation technique. The new distribution of ratings is shown below.

<iframe
  src="assets/dist_of_ratings_after_replacing.html"
  width="800"
  height="600"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -150px;"
></iframe>

One more thing I did was I created an average_rating column. Originally, the data only had individual ratings for each review, so I thought it would be meaningful to group the dataframe by recipe_id, calculate the average rating for each recipe, and impute this data. 

The last step I took in cleaning the data was by organizing the 'nutrition' column. The nutrition column contained information in the form of a string that looked like a list: "[calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates]". I took this information and created separate columns called 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', and 'carbohydrates'. The head of my cleaned and tidy dataframe is shown below.

| name                              |     id |   minutes |   contributor_id | submitted   | tags                              |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |   average_rating |
|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|
| 1 brownies in the world    bes... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |                4 |
| 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |

### Univariate Analysis

So I had a clean dataframe, but what's next? I was looking to answer the question "**Which contributors consistently have the highest average recipe ratings?**" A good way to start analyzing this was to create a column called 'num_contributions' and display its distribution. I created the column 'num_contributions' by grouping the dataframe by 'contributor_id' and counting how many times that contributor appeared. Given the sheer amount of contributors that contributed between 1-99 times, I decided to drop these datapoints when showing the distributions of 'num_contributions'. This made the data much less skewed and also more meaningful to look at, as my EDA question involved looking at contributors that **consistently** have the best ratings. Below is a boxplot showing the distribution of 'num_contributions' for contributors who contributed more than 100 times.

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

After displaying the distribution of single columns, I looked at the distribution of multiple columns. Specifically, I answered my question with a cool interactive plot that looked at the top ten contributors by average rating. I configured the bar plot so that the user can change the minimum threshold for number of contributions for the contributor to be displayed. The reason I started the minimum threshold at 100 is that there are an overwhelming number of contributors with a small amount of contributions and an average rating between 4.9-5 stars. Additionally, I wanted to explore the contributors who were most **consistent**. The number inside of the bar is the nbumber of contributions that contributor has. This plot is shown below.

<iframe
  src="assets/top_10_contributors_by_avg_rating.html"
  width="1000"
  height="700"
  frameborder="0"
  style="margin-top: 20px; margin-bottom: -270px;"

></iframe>

One trend to notice is that as you increase the minimum contribution threshold, the top ten contributos' average recipe ratings decrease. This makes sense because it is extremely difficult to maintain 5 star ratings when you have made more recipes. However, top ten contributors' average recipe ratings always stay above 4.5 stars. Also, we can connect back to seeing a contributor that contributed <span style="color: limegreen;">3,060 recipes</span> (from our box plot of the num_contributions distribution.). That person was contributor 37449 and has an average rating of <span style="color: limegreen;">4.787 stars</span>. That is super impressive!

### Interesting Aggregate Table

Below is a table showing the aggregates of (1) average count of contributions each contributor made in the and (2) the average rating for those contributors. This table helps us visualize the above graph in tabular format. For this table, I made the threshold of minimum contributions 488.5 because that is the third quartile of number of contributions, for contributors with 100+ contributions, as shown in our boxplot (Contributions per Contributor). In my original EDA question, we could define the term "consistent" however we want. In this case, I am defining "consistent" as a contributor who's number of contributions is above the third quartile in the IQR. This table is significant because it also helps us see that, given my definition of a consistent contributor, <span style="color: limegreen;"> contributor 461834 is our top contributor, as they contributed 1680 recipes and had an average rating of 4.85 stars </span>  

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

After that interesting EDA, I decided to shift gears. I decided to predict the 'calories' column of my recipes dataframe. This is a supervised learning regression problem because calories is a numeric variable. I am getting my wisdom teeth taken out soon, and am going to need to eat a lot of soup in the following days. To make it more interesting, I decided to specifically <span style="color: limegreen;">predict the 'calories' column for soup recipes</span>. 

I determined which recipes were soup recipes by filtering my dataframe based on the name of the recipe including the substring "soup". Below is the line of code showing how I did it:

```py
recipes_copy = recipes.copy()
recipes_copy = recipes_copy.loc[recipes_copy['name'].str.contains('soup', case=False, na=False)]
```
Note: The case=False parameter makes it so a case-insensitive search will occur

Now that I have created this smaller dataframe called recipes_copy(), it is important to know how many rows are in the dataframe. After called recipes_copy.shape I saw that there are 9628 rows.

## **Baseline Model**

My baseline model uses two features to predict the 'calories' column. Those two features are 'carbohydrates' and 'sugar', both quantitative features. As a result, I did not have to perform any one-hot encoding. This model is not super sophisticated, and is meant to be a basic introduction to predicting the 'calories' column. Before training and testing my model, one thing I checked was whether there were any NaN values in the calories column, as calories is the target variable. 

```py
print("Number of NaN calories columns:", recipes_copy['calories'].isna().sum())
```

Running this code printed out 0. It appears that the contributors that make soup are good about inputting the number of calories in their recipes.

After creating my train test split, I created a pipline using the LinearRegression() model, as shown below.

```py
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

This graph is a 3-dimensional scatterplot that shows the distribution of actual and predicted values for the calories column. It appears that the model did a decent job, as there is a lot of overlap between the red and blue. Additionally, I used the mean_squared_error function and r2_score functions from sklearn.metrics to calculate the mean-squared error (MSE) of this model. It has a MSE of 26388.78. 

Given that there are 9628 rows in the dataframe this MSE not bad. However, the MSE does not tell the full story. I do not think that this model is very good overall. Firstly, the model relies only on 'carbohydrates' and 'sugar' to predict 'calories'. While these are important factors to indicate calorie content of soups, there are many more features that could be added to improve the predictions. Additionally, while standard scaling helps in handling features with different units or scales, it doesn't address the potential issue of outliers or non-normal distributions of features, which can negatively affect model performance. 

## **Final Model**