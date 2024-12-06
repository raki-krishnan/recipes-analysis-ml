# **Welcome to Raki's Data Analysis!**

## **Introduction**

I am a hungry person, both for knowledge and food (😆). Consequently, I decided that the most interesting data to analyze would be recipes for delicious dishes. The recipes I analyzed are pulled directly from [food.com](https://www.food.com). Each row in my dataframe contains a recipe, and each column contains useful information about the recipes. Displayed below is information about my dataframe, including the shape and column names.

### Shape of the Recipes Dataframe: 

So what was I actually working with once I cleaned the data and was left with my dataframe ready for analysis?

```py
print(recipes.shape)
```
(234429, 24)

The above result from the code means that there are 234,429 rows and 24 columns in my recipes dataframe!

### Exploratory data analysis (EDA) Question: 

**Which contributors consistently have the highest average recipe ratings?**

This is an important question to think about, because if I am ever super hungry in the future (and also learn how to actually cook), I will know which contributors' recipes to look at. After reading through this EDA, you will also know which contributors are the most esteemed on food.com.

### Which columns were most important to my EDA?

```py
print(recipes.columns.to_list())
```
['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients', 'user_id', 'recipe_id', 'date', 'rating', 'review', 'calories', 'total_fat', 'sugar', 'sodium', 'protein', 'saturated_fat', 'carbohydrates', 'average_rating']

Above is a list of all the column names in the cleaned recipes dataframe that I used for analysis.

The columns that are most related to my EDA were 'contributor_id', 'rating', 'average_rating', and a column I later created called 'num_contributions'.

**contributer_id**: A unique way to identify the contributors (also known as primary key). This column is of type integer.
**'rating'**: A single rating of the recipe given by a user. This column is of type integer.
**'average_rating'**: The mean rating that a recipe was given. This column is of type float.
**'num_contributions'**: The number of contributions a contributor made. This column is of type integer.


<iframe
  src="assets/dist_of_ratings_before_replacing.html"
  width="900"
  height="675"
  frameborder="0"
></iframe>


<iframe
  src="assets/dist_of_ratings_after_replacing.html"
  width="900"
  height="675"
  frameborder="0"
></iframe>

| name                              |     id |   minutes |   contributor_id | submitted   | tags                              |   n_steps | steps                             | description                       | ingredients                       |   n_ingredients |          user_id |   recipe_id | date       |   rating | review                            |   calories |   total_fat |   sugar |   sodium |   protein |   saturated_fat |   carbohydrates |   average_rating |
|:----------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------|----------:|:----------------------------------|:----------------------------------|:----------------------------------|----------------:|-----------------:|------------:|:-----------|---------:|:----------------------------------|-----------:|------------:|--------:|---------:|----------:|----------------:|----------------:|-----------------:|
| 1 brownies in the world    bes... | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-t... |        10 | ['heat the oven to 350f and ar... | these are the most; chocolatey... | ['bittersweet chocolate', 'uns... |               9 | 386585           |      333281 | 2008-11-19 |        4 | These were pretty good, but to... |      138.4 |          10 |      50 |        3 |         3 |              19 |               6 |                4 |
| 1 in canada chocolate chip coo... | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-t... |        12 | ['pre-heat oven the 350 degree... | this is the recipe that we use... | ['white sugar', 'brown sugar',... |              11 | 424680           |      453467 | 2012-01-26 |        5 | Originally I was gonna cut the... |      595.1 |          46 |     211 |       22 |        13 |              51 |              26 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |  29782           |      306168 | 2008-12-31 |        5 | This was one of the best brocc... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 |      1.19628e+06 |      306168 | 2009-04-13 |        5 | I made this for my son's first... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |
| 412 broccoli casserole            | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-t... |         6 | ['preheat oven to 350 degrees'... | since there are already 411 re... | ['frozen broccoli cuts', 'crea... |               9 | 768828           |      306168 | 2013-08-02 |        5 | Loved this.  Be sure to comple... |      194.8 |          20 |       6 |       32 |        22 |              36 |               3 |                5 |



<iframe
  src="assets/contributions_per_contributor.html"
  width="700"
  height="800"
  frameborder="0"
></iframe>

<iframe
  src="assets/top_10_contributors_by_avg_rating.html"
  width="1000"
  height="700"
  frameborder="0"
></iframe>


|   contributor_id |   num_contributions |   avg_rating |
|-----------------:|--------------------:|-------------:|
|      2.19261e+06 |                 106 |      5       |
| 306726           |                 115 |      4.98949 |
|      2.39055e+06 |                 360 |      4.97907 |
|      2.68903e+06 |                 139 |      4.97448 |
|      2.21634e+06 |                 140 |      4.97424 |

