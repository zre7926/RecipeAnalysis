# RecipeAnalysis
Data Project for UCSD DSC-80 Course Final Project
Author: Ryan Zhang

## Introduction
Cooking time is one of the most important factors shaping how people choose recipes online. Whether you're a busy student, a working parent, or someone just trying to get dinner on the table, the time a recipe takes can be a dealbreaker. Many users will filter out recipes that take too long before even considering the ingredients or the rating. At the same time, there’s a trade-off: longer recipes may result in more flavorful or satisfying meals, but they can also intimidate or deter users, especially if the payoff isn’t clearly worth the time investment.

In this project, we will be using two datasets - one containing recipes and one containing reviews to those recipes. Both datasets come from food.com. 

The first dataset (`recipe`) has 83782 rows (thus there are that many recipes) and 12 columns, which are

|Column|Description|
|------|-----------|
|`name`|Recipe name|
|`id`|Recipe ID|
|`minutes`|Minutes to prepare recipe|
|`contributor_id`|User ID who submitted this recipe|
|`submitted`|Date recipe was submitted|
|`tags`|Food.com tags for recipe|
|`nutrition`|Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV = “percentage of daily value”|
|`n_steps`|Number of steps in recipe|
|`steps`|Text for recipe steps, in order|
|`description`|User-provided description|
|`ingredients`|Text for ingredients needed|
|`n_ingredients`|Number of ingredients in recipe|

The second dataset (`ratings`) has 731927 rows (thus there are that many recipes) and 5 columns, which are

|Column|Description|
|------|-----------|
|`user_id`|User ID|
|`recipe_id`|Recipe ID|
|`date`|Date of interaction|
|`rating`|Rating given|
|`review`|Review text|

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
Before doing any analysis, I did some cleaning of the datasets and some data visualization. 
First, I merged the two datasets. Specifically, I did a left merge of `recipes` with `ratings` to ensure that every recipe is matched with all of its reviews. This step is very important since it leaves us with just one dataframe to work with throughout the project.

Then, I replaced all the zero ratings with `np.nan` since a standard rating should be in the range 1 to 5. Therefore, ratings of 0 should be considered missing to avoid skewing averages. 

The `nutrition` column was originally stored as a stringified list, which I split into usable numeric columns. This enables quantitative nutritional analysis. Without this, we couldn’t use nutritional features in modeling or compare recipes by dietary content.

Finally, I computed the average rating of each recipe and created a separate column `avg_rating`. By averaging ratings, it helps us summarize a recipe's reception. 

After these steps of data cleaning, `df` has 234429 rows and 25 columns.
The first few rows look like this (I didn't include some of the columns since they are less significant and long):

| name                                 |     id |   minutes |   contributor_id | submitted   |   n_steps |   n_ingredients |          user_id |   rating |   calories |   avg_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|----------:|----------------:|-----------------:|---------:|-----------:|-------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  |        10 |               9 | 386585           |        4 |      138.4 |            4 |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  |        12 |              11 | 424680           |        5 |      595.1 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  |         6 |               9 |  29782           |        5 |      194.8 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  |         6 |               9 |      1.19628e+06 |        5 |      194.8 |            5 |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  |         6 |               9 | 768828           |        5 |      194.8 |            5 |

### Univariate Analysis
For an univariate analysis, I examined the distribution of cooking time within the dataset. It seems that the distribution is skewed to the right with the center being around 30-40 minutes. 
<iframe
  src="assets/univariateplot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>
Interesting, there are many outliers. In fact, one recipe takes 1 million minutes to cook and it's title is 'how to preserve a husband'.

### Bivariate Analysis

<iframe
  src="assets/bivariateplot.html"
  width="1000"
  height="400"
  frameborder="0"
></iframe>

### Interesting Aggregates




## Assessment of Missingness

### NMAR Analysis

### Missingness Dependency

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

hihi