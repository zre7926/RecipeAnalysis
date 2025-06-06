# RecipeAnalysis
Data Project for UCSD DSC-80 Course Final Project
Author: Ryan Zhang

## Introduction
Cooking time is one of the most important factors shaping how people choose recipes online. Whether you're a busy student, a working parent, or someone just trying to get dinner on the table, the time a recipe takes can be a dealbreaker. Many users will filter out recipes that take too long before even considering the ingredients or the rating. At the same time, there’s a trade-off: longer recipes may result in more flavorful or satisfying meals, but they can also intimidate or deter users, especially if the payoff isn’t clearly worth the time investment.

In this project, we will be using two datasets - one containing recipes and one containing reviews to those recipes. Both datasets come from food.com. 

The first dataset (`recipe`) has 83782 rows (thus there are that many recipes) and 12 columns, which are

|Column|Description|
|------|-----------|
|name|Recipe name|
|id|Recipe ID|
|minutes|Minutes to prepare recipe|
|contributor_id|User ID who submitted this recipe|
|submitted|Date recipe was submitted|
|tags|Food.com tags for recipe|
|nutrition|Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV = “percentage of daily value”|
|n_steps|Number of steps in recipe|
|steps|Text for recipe steps, in order|
|description|User-provided description|
|ingredients|Text for ingredients needed|
|n_ingredients|Number of ingredients in recipe|

The second dataset (`ratings`) has 731927 rows (thus there are that many recipes) and 5 columns, which are

|Column|Description|
|------|-----------|
|user_id|User ID|
|recipe_id|Recipe ID|
|date|Date of interaction|
|rating|Rating given|
|review|Review text|



## Data Cleaning and Exploratory Data Analysis

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis

hihi