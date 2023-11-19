# Food Recipe Analysis Project
**Authors**: Hikaru Isayama, Julia Jung

## Project Overview
This data science project investigates the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted since 2008 on food.com.

---

## Introduction and Question Identification

With the continuing concerns about health in the US, having a more accurate understanding of what affects calorie intake helps maintain a healthy weight, as consuming too many calories can lead to weight gain and associated health issues like obesity, diabetes, and heart problems. This project tests a new indicator - number of ingredients in a recipe - to see whether or not this value has any impact on the amount of calories in the meal. In other words, we will investigate whether or not a greater number of ingredients in a recipe correlates to higher calories, by analyzing the relationship between the number of ingredients and the amount of calories.

### Datasets used (food.com)

The first dataset we used in this project contains recipes posted on food.com since 2008, with 83782 rows representing each recipe posted, and each row consisting these 12 columns of descriptions: 

|Column	                 |Description|
|---                     |---        |
|`'name'`            |Recipe name|
|`'id'`	                 |Recipe ID|
|`'minutes'`	         |Minutes to prepare recipe|
|`'contributor_id'`	     |User ID who submitted this recipe|
|`'submitted'`	            | Date recipe was submitted|
|`'tags'`	              |Food.com tags for recipe|
|`'nutrition'`	          |Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein    (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”|
|`'n_steps'`	          |Number of steps in recipe|
|`'steps'`	              |Text for recipe steps, in order|
|`'description'`	     | User-provided description|
|`'ingredients'`	              |List of ingredients|
|`'n_ingredients'`	     | Number of ingredients|

The second dataset we used on this project contains reviews and ratings submitted for the recipes in the first dataset posted on food.com since 2008. This dataset contains 731927 rows representing each individual review, with these 5 columns of descriptions:

|Column|Description|
|---|---|
|`'user_id'`	|User ID|
|`'recipe_id'`	|Recipe ID|
|`'date'`	|Date of interaction|
|`'rating'`	|Rating given|
|`'review'`	|Review text|

As this project investigates the relationship between the number of ingredients and the amount of calories (#) in each recipe, the two columns we will reference the most are:

1. The `'nutrition'` column of the first dataset. This column contains information about the recipe in the following format: <br><br>`'[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]'`<br><br>To allow the calories (#) to be more accessible, we decided to create a new column called `'calories'` in the new merged dataset, which we will reference instead of the entire `'nutrition'` column

2. The `'n_ingredients'` column of the first dataset. This column contains the value for the number of ingredients in the recipe, which we use to compare with the calorie value of that recipe.

---

## Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning

As mentioned above, we were provided two different datasets to use for our study. In order to combine the datasets and extract the data required for this study, we followed these steps to clean the given datasets:

*Project guideline steps:*
1. Left merge the recipes and interactions datasets together.
- This step allows access to information in both datasets in a new singular dataframe.
2. In the merged dataset, fill all ratings of 0 with '`'np.nan'` .
- Examining the interactions on food.com, we discovered that ratings of 0 in the dataset indicates how the reviewer decided not to give a rating, rather than giving a rating of 0 (which is impossible, as the lowest rating is 1 star). Therefore, by replacing 0’s with np.nan, we are now able to perform calculations more accurately.
3. Find the average rating per recipe, as a Series.
- Using the accurate ratings thanks to the previous step, we created a series with the column name  `'avg_rating'` consisting of the agerage rating for each recipe (which we ultimately did not use for our study).
4. Add this Series containing the average rating per recipe back to the recipes dataset.
- Added the series created in the previous step to our merged dataframe. Again, we ultimately did not use this data for our study.

*Additional steps for study:*
5. Convert datatype for each column.
- The some columns in the raw dataset contain values that look like lists but are strings, values that look like dates but are strings, etc. This step converts these columns into more usable data types to perform calculations. For our study, we converted the `'tags'`, `'nutrition'`, `'steps'`, and  `'ingredients'` columns from strings to lists. 
6. Create new `'calorie'` column
- Since we are using the calorie value of the `'nutrition'` column in this study, we decided to create this new column to make this value even more accessible (as described in the introduction). 
7. Create new shopping_cart column
- To allow for an even more in-depth analysis of our research question, we decided to categorize the value for the number of ingredients. This new column will allow us to investigate trends for different intervals of the number of ingredients. For our categories and intervals, we used: 0-5 ingredients for `'near empty'`, 5-10 ingredients for `'light'`, 10-15 ingredients for `medium'`, 15-20 ingredients for `'heavy'`, 20-25 ingredients for `'full'`, and 25+ ingredients for `'overflowing'` (with the first value inclusive, last value exclusive). 
8. Filter + select columns to use for study
- For our project, we will extract the following columns required to perform our calculations and tests later in the study: `'n_ingredients'`, `'calorie'`,  `'shopping_cart'`. 

| id    | n_ingredients | calories | shopping_cart |
|------:|:--------------|:---------|--------------:|
|275022 | 7             | 386.1    |   light       |
|275024 | 8             | 377.1    |   light       |  
|275026 | 9             | 326.6    |   light       |
|275030 | 9             | 577.7    |   light       |
|275032 | 9             | 386.9    |   light       |


### Univariate Analysis

To display the distribution of the number of ingredients, and the distribution of calories of each recipe in our cleaned dataset, we decided to use histograms for our univariate graphical analysis. 

<iframe src="assets/ingredients_hist.html" width=800 height=600 frameBorder=0></iframe>

Above is a histogram that shows the distribution of the number of ingredients per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 5-15 ingredient range, with most of the outliers being greater than the majority – leading to a right-skewed distribution. 

<iframe src="assets/calories_hist.html" width=800 height=600 frameBorder=0></iframe>

Above is a histogram that shows the distribution of the number of calories per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 0-500 calorie range, with again most of the outliers being on the greater end – leading to a right-skewed distribution. 

### Bivariate Analysis

To create a visual for the relationship between the number of ingredients and the amount of calories (#), we decided to use a boxplot to display the amount of calories against `'shopping_cart'`. We chose to use this graph for the purpose of easily being able to identify the five-number summary and any outliers for each `'shopping_cart'` value.

<iframe src="assets/bivariate_box_plot.html" width=800 height=600 frameBorder=0></iframe>

Here are the boxplots as described above – with the y-axis measuring the amount of calories, and the x-axis representing each value in the column `'shopping_cart'`. We can observe that there appears to be an increase in the values for quartile 1, median, quartile 3, and max (ignoring outlisers) as the size of the shopping cart increases (with the exception of the `'light'` variable). This information suggests that there may be a level of positive correlation between the size of `'shopping_cart'` (which represents the number of ingredients) and the amount of calories per recipe.

### Interesting Aggregates

|   shopping_cart| mean<br><br>| date                |        |
|                |   year      |
|---------------:|:------------|:--------------------|-------:|
|          138.4 | snack       | 2008-11-19 00:00:00 |   2008 |
|          595.1 | lunch       | 2012-01-26 00:00:00 |   2012 |
|          194.8 | snack       | 2008-12-31 00:00:00 |   2008 |
|          194.8 | snack       | 2009-04-13 00:00:00 |   2009 |
|          194.8 | snack       | 2013-08-02 00:00:00 |   2013 |

	mean	median	min	max
calories	calories	calories	calories
shopping_cart				
near empty	291.411553	180.6	0.0	2987.6
light	356.235173	269.6	0.0	2999.8
medium	429.935659	347.7	0.3	2998.4
heavy	523.604491	438.1	2.5	2978.2
full	611.110988	510.8	24.4	2766.5
overflowing	722.479021	630.0	77.2	2935.9

---
## Assessment of Missingness

### NMAR Analysis

In our dataset, there are several columns that contain missing values, which could depend on a variety of factors. 

One such column that possibly could be NMAR is the `'description'` column. The missingness could be due to the reviewer’s availability of time or their pride about the recipe, which is not measured in our dataset. For example, reviewers who have a lot of time at their hands may write descriptions for the recipes they submit, while those who do not may not write one. Similarly, reviewers who are more proud about the recipe they submit may be more motivated to write a description, while those who are not may omit it entirely. As these values are not recorded in our dataset, the `'description'` column is possibly an NMAR.

We can change the `'description'` column from NMAR to MAR if we include variables that measure the free time reviewers have, or how proud they are of the recipe. For example, if there was a rating scale of how proud they are about the recipe they submit, the missingness of the `'description'` column could be influenced by the low values measured on this new variable. 

### Missingness Dependency

In the datasets, we noticed that there were several columns with missing values – with the `'description'` column being one of them. For our missingness analyses, we decided to investigate whether or not the missingness of `'description'` does/does not depend on `'n_steps'` (number of steps), and on `'n_ingredients.'` Running permutation tests to study the missingness, we discovered that:

#### **1. The missingness of `'description'` does depend on `'n_steps'` (MCAR).**

**Null Hypothesis**: The missingness of `'description'` does not depend on `'n_steps'` 

**Alternative Hypothesis**: The missingness of `'description'` does depend on `'n_steps'`

Below, we have a overlayed histogram which compares the distribution of `'n_steps'` when `'description'` is missing and when it is not.

<iframe src="assets/missing_desc_steps.html" width=800 height=600 frameBorder=0></iframe> 

For our initial observation of this distribution, we can see that the two distripution are fairly similar, with most of the distribution overlapping.

For our test statistic, we chose to use the absolute difference in means as `'n_steps'` is a numerical variable and since our hypothesis don't include a direction. Running 1000 simulations of a permutation test by shuffling the `'description'` column, we plotted the distribution of the test statistics in the graph below: 

<iframe src="assets/empirical_plot_steps.html" width=800 height=600 frameBorder=0></iframe> 

In this distribution, the red line represents the observed test statistic of our original dataset. From our simulations, we found that the p-value is 0.24 – which is reflected on the distribution as there is a noticable proportion of values to the right of the observed statistic.

Therefore, since our p-value is greater then the significance level of 0.05, we **fail to reject** the null hypothesis – meaning that the missingness in the `'description'` column **is not** dependent on `'n_steps'`**(MCAR)**.

#### **2. The missingness of `'description'` does not depend on `'n_ingredients'` (MAR).**

**Null Hypothesis**: The missingness of `'description'` does not depend on `'n_ingredients'`

**Alternative Hypothesis**: The missingness of `'description'` does depend on `'n_ingredients'`

Below, we have another overlayed histogram which compares the distribution of `'n_ingredients'` when `'description'` is missing and when it is not.

<iframe src="assets/missing_desc_ingred.html" width=800 height=600 frameBorder=0></iframe> 

For our initial observation of this distribution, we can see that the two distripution are fairly similar, with most of the distribution overlapping.

For our test statistic, we again chose to use the absolute difference in means as `'n_ingredients'` is a numerical variable and since our hypothesis don't include a direction. Running 1000 simulations of a permutation test by shuffling the `'description'` columns, we plotted the distribution of the test statistics in the graph below: 

<iframe src="assets/empirical_plot_ingred.html" width=800 height=600 frameBorder=0></iframe> 

In this distribution, the red line represents the observed test statistic of our original dataset. From our simulations, we found that the p value is 0.01 – which is reflected on the distribution as there are little to no values to the right of the observed statistic.

Therefore, since our p-value is smaller than the significance level of 0.05, we **reject** the null hypothesis – meaning that the missingness in the `'description'` column **is** dependent on `'n_ingredients'` **(MAR)**.

---
## Hypothesis Testing

### Hypothesis Testing

Our project investigates the relationship between the number of ingredients (#) and amount of calories from the recipes. During our bivariate analysis, we observed an increase in the amount of calories as the size of the shopping cart increased. For our hypothesis testing, we will test whether or not this observation will also be reflected in the results from the test.

We will define each of the following:

**Null Hypothesis**: the values in the distribution of the amount of calories recorded when `'shopping_cart'` is `'near_empty'` and `'overflowing'` 
comes from the same population, and the observed differences in our samples are due to random chance.
- We use `'near_empty'` and `'overflowing'` as the two distributions to compare, as it allows us to efficently compare the distributions of a recipe with a small number of ingredients against a recipe with a large number of ingredients.

**Alternative Hypothesis**: the values in the distribution of the amount of calories recorded when `'shopping_cart'` is `'overflowing'` is greater than when `'shopping_cart'` is `'near_empty'`. The observed difference in our samples cannot be explained by random chance alone.
- Since we observed how the amount of calories seemed to increase as the `'shopping_cart'` becomes larger in our bivariate analysis, we will use a directional alternative hypothesis to test this observation.

**Test Statistic**: the difference in group means, calculated by: *mean calories of recipes that are considered `'overflowing'`* − *mean calories of recipes that are considered `'near_empty'`*. 
- We use the difference in group means and not the absolute difference since our alternative hypothesis indicates direction.

**Significance Level**: we will use a significance level of 0.05 for our conclusion to ensure accuracy.
- a common significance level for most hypothesis tests.

The graph below displays the empirical distribution of our test statistic in 1000 permutations, under the null hypothesis stated above. 

<iframe src="assets/empirical_plot_ingred.html" width=800 height=600 frameBorder=0></iframe> 

From the graph above, we can see that the observed statistic is most likely not coincidental, as there were no cases in the 1000 simulation that resulted a test statistic equal to or more extreme than our observed statistic. From the simulation, we also found the p-value to be 0.0, which is less than our significane level of 0.05 for this hypothesis test. Therefore, we can reject the null hypothesis: the results of the test strongly suggest that the observed differences in our samples are not due to random chance, and that the values in the distribution of the amount of calories recorded when `'shopping_cart'` is `'overflowing'` tends to be greater than when `'shopping_cart'` is `'near_empty'`.
















