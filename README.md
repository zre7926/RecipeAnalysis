# Recipe Prep Times: Analysis, Testing, and Predicting
Data Science Project @ UCSD

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
For univariate analysis, I examined the distribution of cooking time within the dataset. It seems that the distribution is skewed to the right with the center being around 30-40 minutes. I noticed that there are a few outliers. In fact, one recipe takes 1 million minutes to cook and it's title is 'how to preserve a husband'.
<iframe
  src="assets/univariateplot.html"
  width="800"
  height="430"
  frameborder="0"
></iframe>

### Bivariate Analysis
For bivariate analysis, I examined the relationship between cooking time and average rating by plotting a scatter plot of the two columns. It seems that most data points are around the top left of the plot, which means high rating and less time. 
<iframe
  src="assets/bivariateplot.html"
  width="1000"
  height="430"
  frameborder="0"
></iframe>

### Interesting Aggregates
I created a pivot table to see the relationship between the number of steps, the number of ingredients, and the time taken. The left represents different groups of n_steps while the top represents different groups of n_ingredients. The values within the table are the median cooking times for each of these groups. 

There is a clear pattern where the more steps and ingredients a recipe has, the longer it takes. This is because as we move down and right along the table, the values become larger. 

|ingredients_band|≤5|6–10|11–15|16–20|>20|
|steps_band||||||
|:-------------|----:|----:|----:|----:|----:|
|≤5|10|20|33|50|90|
|6–10|25|35|40|50|60|
|11–15|35|40|45|55|70|
|16–20|40|50|55|60|75|
|>20|55|60|65|80|95|

## Assessment of Missingness
### NMAR Analysis
The `description` column is plausibly Not Missing At Random (NMAR) because the very decision to leave it blank often depends on qualities of the unwritten text itself. For example, authors may skip a blurb when the title already conveys the full idea, when the back-story is too personal or culturally sensitive, or when they feel their writing would be sub-par; all of these motives depend on information that is not recorded elsewhere in the dataset. To re-frame the mechanism as MAR, we would need variables that capture those hidden motives. For example, an “author-reason” column collected at submission time (self-explanatory title, privacy concerns, language barrier). Conditioning on such observed factors would let us explain the missingness without referencing the unseen description content, satisfying the MAR assumption.

### Missingness Dependency
On the other hand, the column `rating` has non-trivial missingness to analyze. Thus, I will be performing permutation tests to determine whether there is dependency of the missingness on other columns. Specifically, I will test the numerical columns `minutes` and `n_steps`. 

First Permutation Test:

Null Hypothesis: The distribution of `minutes` when `rating` is missing is the same as the distribution of `minutes` when `rating` is not missing.

Alternate Hypothesis: The distribution of `minutes` when `rating` is missing is not the same as the distribution of `minutes` when `rating` is not missing.

The test statistic used is the difference in means and the significance level is 0.05.

<iframe
  src="assets/minutesmissing.html"
  width="1000"
  height="430"
  frameborder="0"
></iframe>
p-value = 0.117 >= 0.05. Thus, we fail to reject the null hypothesis. The missingness of `rating` does not depend on amount of time to cook the recipe (evidence for MCAR). 

Second Permutation Test:

Null Hypothesis: The distribution of `n_steps` when `rating` is missing is the same as the distribution of `n_steps` when `rating` is not missing.

Alternate Hypothesis: The distribution of `n_steps` when `rating` is missing is not the same as the distribution of `n_steps` when `rating` is not missing.

The test statistic used is the difference in means and the significance level is 0.05.

<iframe
  src="assets/n_stepsmissing.html"
  width="1000"
  height="430"
  frameborder="0"
></iframe>
p-value = 0.000 < 0.05. Thus, we reject the null hypothesis. The missingness of `rating` does depend on the number of steps (evidence for MAR). 

In conclusion, my permutation tests indicate that `rating` missingness is Missing At Random (MAR): it shows clear dependence on `n_steps` (p = 0.000 < 0.05) but no detectable dependence on `minutes` (p = 0.117 > 0.05).

## Hypothesis Testing

I wondered whether patience pays off in recipe ratings: do dishes that take longer earn different average scores than quick fixes? To answer this, we compared the mean user rating of “long” recipes to that of “short” recipes. A label-shuffling permutation test, using the difference in means as the statistic, lets us make this comparison without relying on normality assumptions.

Null Hypothesis: The mean rating for “long” recipes (cooking time > median) equals the mean rating for “short” recipes (cooking time <= median).

Alternative hypothesis: The two means differ.

The test statistic used is the difference in means and the significance level is 0.05.

<iframe
  src="assets/permtest.html"
  width="1000"
  height="430"
  frameborder="0"
></iframe>
p-value = 0.000 < 0.05. Thus, we reject the null hypothesis. Thus, there is strong evidence that average ratings for longer recipes differ from those for shorter ones, with the observed difference suggesting slightly lower ratings for longer dishes.

## Framing a Prediction Problem
I will be doing a multiclass classification with the goal of predicting whether a recipe's cooking time is short (≤ 25 min), medium (26–50 min), or long (> 50 min).

Thus, the response variable is `time_cat`, a three-level categorical version of minutes.
I chose this response variable because raw `minutes` is extremely right-skewed and noisy. Dividing into three ranges produces a stabler signal and an answer that readers actually act on.

The evaluation metric I will be using is the F1-score. I chose the F1-score over accuracy because it gives information on the kinds of errors the classifier makes since it combines precision and recall. 

All the features in our model — `n_steps`, `n_ingredients`, `calories`, `uses_oven` or `desc_length` — come directly from the recipe’s content as soon as it’s published, so they would be available at the time of prediction. In contrast, something like `avg_rating` doesn’t exist until users start cooking and rating, so it’s excluded. By restricting ourselves to only those recipe‐text fields, we ensure the model never “peeks” at future information and truly predicts prep‐time based solely on what’s known up front.

## Baseline Model
My baseline model is a RandomForest Classifier using the feature `n_steps` (the number of steps in the recipe) and `n_ingredients` (the number of ingredients needed). I chose these two features because recipes with more steps and more ingredients tend to lead to longer cooking time from my experience. Intuitively, it seems that these two features will work. Both are quantitative variables so there was no need for any encodings or transformations. 

I split the data into train/test groups with a 80/20 split and fitted RandomForestClassifier() with default settings. 

The F1 score of this model was 0.52. Specifically, the F1 score for the groups are 0.45, 0.46, and 0.64 for the short, medium, and long groups respectively. Thus, it seems that our model is good at capturing recipes that take >50 mins while it is not as good for recipes that take less. The F1 score is well above random-guessing but still leaves a lot of room for improvement. Thus, I will attempt to build a stronger classifier that incorporates richer features. 

## Final Model
To boost performance over the two-feature baseline, I added three new features that capture additional notions of complexity and cooking style:

- calories (quantitative). Higher‐calorie dishes often require richer ingredients, longer prepping, or additional steps. Even if two recipes have the same step‐count and ingredient‐count, a 50 calorie salad behaves very differently from a 1000 calorie casserole.

- uses_oven (binary; nominal). Baking or roasting steps (whenever the raw instructions mention “oven”) introduces a lot of time, especially when you have to preheat the oven (a common step). 

- desc_length (quantitative). Recipes with longer author descriptions tend to be more involved: they often include back‐story, detailed tips, or multi‐part cooking instructions that don’t always appear in the numbered steps.

These three, in addition to the baseline n_steps and n_ingredients, give a richer picture. Since all of my features are either quantitative or binary already, I didn't do any encodings or transformations. 

For the model, I continued to use a RandomForestClassifier with the five features mentioned. I split the data into train/test groups with a 80/20 split (I made sure that the split is the same as the baseline model so that I can compare the metrics more effectively). 

I performed GridSearchCV (5-fold CV) on the training split, optimizing F1 score. I decided to search over:
- n_estimators ∈ {50, 100, 150, 200, 250} – a larger forest usually lowers variance, and after adding features I anticipated that more trees would improve stability.
- max_depth ∈ {None, 10, 30, 60} – shallow trees help avoid overfitting on numeric counts, while deeper trees can capture subtler interactions.
All hyperparameter combinations were tested on the same train set used by the baseline, so comparisons remain fair. The best parameter came out to be no max depth and 150 estimators. 

The F1 score of this model was 0.89. Specifically, the F1 score for the groups are 0.88, 0.87, and 0.92 for the short, medium, and long groups respectively. There is a clear improvement from the baseline model, especially for the short and medium groups. The model is still better at predicting long times. I can confidently attribute the performance gain to feature engineering, not to lucky data partitions since I used the same divide. 

## Fairness Analysis
I will determine whether my model is fair amongst different groups. Specifically, the two groups are
- Group X (“long” descriptions): recipes with desc_length > median length
- Group Y (“short” descriptions): recipes with desc_length ≤ median length
The evaluation metric will be accuracy. 

I will conduct a permutation test to determine whether the model is fair amongst these two groups. 

Null Hypothesis: Our model is fair. The accuracy of group X and group Y are the same. 

Alternate Hypothesis: Our model is unfair. The accuracy of group X is larger than the accuracy of group Y (recipes with long descriptions are predicted more accurately)

The test statistic used is the difference in accuracy (accuracy of X - accuracy of Y) and the significance level is 0.05.

<iframe
  src="assets/fairness.html"
  width="1000"
  height="430"
  frameborder="0"
></iframe>
p-value = 0.001 < 0.05. Thus, we reject the null hypothesis. That is, my model predicts recipes with longer descriptions more accurately. 