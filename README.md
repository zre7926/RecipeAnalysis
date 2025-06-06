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
Before doing any analysis, I did some cleaning of the datasets and some data visualization. 
First, I merged the two datasets. Specifically, I did a left merge of `recipes` with `ratings` to ensure that every recipe is matched with all of its reviews. This step is very important since it leaves us with just one dataframe to work with throughout the project.

Then, I replaced all the zero ratings with `np.nan` since a standard rating should be in the range 1 to 5. Therefore, ratings of 0 should be considered missing to avoid skewing averages. 

The `nutrition` column was originally stored as a stringified list, which I split into usable numeric columns. This enables quantitative nutritional analysis. Without this, we couldn’t use nutritional features in modeling or compare recipes by dietary content.

Finally, I computed the average rating of each recipe and created a separate column `avg_rating`. By averaging ratings, it helps us summarize a recipe's reception. 

After these steps of data cleaning, `df` has 234429 rows.
The first few look like this:
|Unnamed:0|name|id|minutes|contributor_id|submitted|tags|nutrition|n_steps|steps|description|ingredients|n_ingredients|user_id|recipe_id|date|rating|review|calories|total_fat|sugar|sodium|protein|saturated_fat|carbohydrates|avg_rating|
|---------|----|--|--------|---------------|----------|-----|----------|--------|------|------------|-----------|--------------|--------|----------|------|------|--------|--------|----------|-----|------|-------|---------------|--------------|-----------|
|0|1browniesintheworldbestever|333281|40|985201|2008-10-27|['60-minutes-or-less','time-to-make',...]|[138.4,10.0,50.0,3.0,3.0,19.0,6.0]|10|['heattheovento350fandarrangetherackinthemiddle',...]|thesearethemost;chocolatey,moist,rich,dense,fudgy,deliciousbrowniesthatyou'll...|['bittersweetchocolate','unsaltedbutter',...]|9|386585|333281|2008-11-19|4|Thesewereprettygood,buttookforevertobake...|138.4|10|50|3|3|19|6|4|
|1|1incanadachocolatechipcookies|453467|45|1848091|2011-04-11|['60-minutes-or-less','time-to-make',...]|[595.1,46.0,211.0,22.0,13.0,51.0,26.0]|12|['pre-heatoventhe350degreesf',...]|thisistherecipethatweuseatmyschoolcafeteriaforchocolatechipcookies...|['whitesugar','brownsugar','salt',...]|11|424680|453467|2012-01-26|5|OriginallyIwasgonnacuttherecipeinhalf...|595.1|46|211|22|13|51|26|5|
|2|412broccolicasserole|306168|40|50969|2008-05-30|['60-minutes-or-less','time-to-make',...]|[194.8,20.0,6.0,32.0,22.0,36.0,3.0]|6|['preheatovento350degrees',...]|sincetherearealready411recipesforbroccolicasserolepostedto"zaar",...|['frozenbroccolicuts','creamofchickensoup',...]|9|29782|306168|2008-12-31|5|ThiswasoneofthebestbroccolicasserolesthatIhaveevermade...|194.8|20|6|32|22|36|3|5|
|3|412broccolicasserole|306168|40|50969|2008-05-30|['60-minutes-or-less','time-to-make',...]|[194.8,20.0,6.0,32.0,22.0,36.0,3.0]|6|['preheatovento350degrees',...]|sincetherearealready411recipesforbroccolicasserolepostedto"zaar",...|['frozenbroccolicuts','creamofchickensoup',...]|9|1196280|306168|2009-04-13|5|Imadethisformyson'sfirstbirthdaypartythisweekend...|194.8|20|6|32|22|36|3|5|
|4|412broccolicasserole|306168|40|50969|2008-05-30|['60-minutes-or-less','time-to-make',...]|[194.8,20.0,6.0,32.0,22.0,36.0,3.0]|6|['preheatovento350degrees',...]|sincetherearealready411recipesforbroccolicasserolepostedto"zaar",...|['frozenbroccolicuts','creamofchickensoup',...]|9|768828|306168|2013-08-02|5|Lovedthis.Besuretocompletelythawthebroccoli...|194.8|20|6|32|22|36|3|5|


## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

hihi