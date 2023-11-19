# Food Recipe Analysis Project
**Authors**: Hikaru Isayama, Julia Jung
Data science project from the dsc80 course as UCSD. 

This data science project investigates the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted since 2008 on food.com.
---

## Introduction and Question Identification

With the continuing concerns about health in the US, having a more accurate understanding of calorie intake helps maintain a healthy weight, as consuming too many calories can lead to weight gain and associated health issues like obesity, diabetes, and heart problems. This project investigates a new indicator (number of ingredients) to see whether or not this value has any impact on the amount of calories in the recipe.

### Datasets used (food.com)

The first dataset we used on this project contains recipes posted on food.com since 2008, containing 83782 rows representing each recipe posted, with each row consisting these 12 columns of descriptions: 

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

As this project investigates the relationship between the number of ingredients and the amount of calories (#) in each recipe, the two columns we referenced the most were:

1. The 'nutrition' column of the first dataset. This column contains information about the recipe in the following format: <br><br>`'[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]'`<br><br>To allow the calories (#) to be more accessible, we decided to create a new column called calories in the merged dataset, which we will reference instead of the entire 'nutrition' column

2. The  ‘n_ingredients’ column of the first dataset. This column contains the value for the number of ingredients in the recipe, which we use to compare with the calorie value of that recipe.

---

## Cleaning and EDA (Exploratory Data Analysis)

### Data Cleaning

As mentioned above, we were provided two different datasets to use for our study. In order to combine the datasets and extract the data required for this study, we followed this step to clean the given datasets:

*Project guideline steps:*
1. Left merge the recipes and interactions datasets together.
- This step allows access to information in both datasets in a new singular dataframe.
2. In the merged dataset, fill all ratings of 0 with '`'np.nan'` .
- Examining the recipes on food.com, we discovered that ratings of 0 in the dataset indicates how the reviewer decided not to give a rating, rather than giving a rating of 0 (which is impossible, as the lowest rating is 1 star). Therefore, by replacing 0’s with np.nan, we are able to perform calculations more accurately.
3. Find the average rating per recipe, as a Series.
- Using the accurate ratings thanks to the previous step, we created a series with the column name  `'avg_rating'`, which we ultimately did not use for our study.
4. Add this Series containing the average rating per recipe back to the recipes dataset.
- Added the series created in the previous step to our merged dataframe Again, we ultimately did not use this data for our study.

*Additional steps for study:*
5. Convert datatype for each column.
- The some columns in the raw dataset contain values that look like lists but are strings, values that look like dates but are strings, etc. This step converts these columns into more usable data types to perform calculations. For our study, we converted the `'tags'`, `'nutrition'`, `'steps'`, and  `'ingredients'` columns from strings to lists. 
6. Create new `'calorie'` column
- Since we are using the calorie value of the `'nutrition'` column in this study, we decided to create this new column to make this value even more accessible (as described in the previous section). 
7. Create new shopping_cart column
- To allow for even more in-depth analysis of our research question, we decided to categorize the value for the number of ingredients in each recipe. This new column will allow us to investigate trends for the different intervals of the number of ingredients. For our categories and intervals, we used: 0-5 ingredients for `'near empty'`, 5-10 ingredients for `'light'`, 10-15 ingredients for `medium'`, 15-20 ingredients for `'heavy'`, 20-25 ingredients for `'full'`, and 25+ ingredients for `'overflowing'`. 
8. Filter + select columns to use for study
- For our project, we will extract the following columns required to perform our calculations and tests later in the study: `'n_ingredients'`, `'calorie'`,  `'shopping_cart'`. 

### Univariate Analysis

To display the distribution of the number of ingredients, and the distribution of  calories of each recipe in our cleaned dataset, we decided to use histograms for our graphical analysis. 

<iframe src="assets/ingredients_hist.html" width=800 height=600 frameBorder=0></iframe>

Above is a histogram that shows the distribution of the number of ingredients per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 5-15 ingredient range, with most of the outliers more than the majority – leading to a right- skewed distribution. 

<iframe src="assets/calories_hist.html" width=800 height=600 frameBorder=0></iframe>

Above is another histogram that shows the distribution of the number of calories per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 0-500 calorie range, with again most of the outliers being on the greater end – leading to a right-skewed distribution. 

### Bivariate Analysis

To create a visual for the relationship between the number of ingredients and the amount of calories (#), we decided to use a boxplot plotting the amount of calories against `'shopping_cart'`. We chose to use this graph for the purpose of easily being able to identify the min, quartile 1, median, quartile 3, max, and any outliers for each `'shopping_cart'` value.

<iframe src="assets/bivariate_box_plot.html" width=800 height=600 frameBorder=0></iframe>

Here are the boxplots as described above – with the y-axis measuring the amount of calories, and the x-axis representing each value in the column `'shopping_cart'`. We can observe that there appears to be an increase in the values for quartile 1, median, quartile 3, and max as the size of the shopping cart increases (with the exception of the `'light'` boxplot). This information suggests that there may be a level of positive correlation between the size of `'shopping_cart'` (which represents the number of ingredients) and the amount of calories per recipe.

### Interesting Aggregates


---
## Assessment of Missingness

### NMAR Analysis

In our dataset before we filtered, the `'description'` column could possibly be NMAR. This could be due to the reviewer’s interest in investing time or pride about the recipe, which is not measured in our dataset. For example, reviewers who have a lot of time at their hands may write descriptions for the recipes they submit, while those who do not may not. Similarly, reviewers who are more proud about the recipe they submit may be more motivated to write a description, while those who are not may omit it entirely. However, these values are not recorded in our dataset, therefore making the `'description'` column a NMAR.

We can change the `'description'` column from NMAR to MAR if we include variables that measure the free time reviewers have, or how proud they are of the recipe. For example, if there was a rating scale of how proud they are about the recipe they submit, the missingness of the `'description'` column could be influenced by the low values measured on this new variable. 

### Missingness Dependency

In the datasets, we noticed that there were several columns with missing values – with the `'description'` column being one of them. For our missingness analyses, we decided to investigate whether or not the missingness of `'description'` does/does not depend on `'n_steps'` (number of steps), and on `'n_ingredients.'` Running permutation tests to study the missingness, we discovered that:

#### **The missingness of rating **does** depend on `'n_steps'` (MCAR).**

**Null Hypothesis: The missingness of description does not depend on `'n_steps'`** <br><br> **Alternative Hypothesis: The missingness of description does not depend on `'n_steps'`** 

Below, we have a overlayed histogram which compares the distribution of `'n_steps'` when `'description'` is NaN and when it is not.

<iframe src="assets/missing_desc_steps.html" width=800 height=600 frameBorder=0></iframe> 

For our test statistic, we chose to use the absolute difference in means as `'n_steps'` is a numerical variable. Running 1000 simulations of a permutation test by shuffling the `'description'` columns, we plotted the distribution of the test statistics here: 

<iframe src="assets/empirical_plot_steps.html" width=800 height=600 frameBorder=0></iframe> 

In the distribution above, the red line indicates the observed test statistic of our original dataseet. From our simulations, we found that the p value is 0.24 – which is reflected on the distribution as there is a noticable proportion of values to the right of the observed statistic.

Therefore, using a significance level of 0.05, we **fail to reject** the null hypothesis – meaning that the missingness in the `'description'` column **is not** dependent on `'n_steps'`**(MCAR)**.

#### **The missingness of rating does not depend on `'n_ingredients'` (MAR).**

**Null Hypothesis: The missingness of description does not depend on `'n_ingredients'`** 

**Alternative Hypothesis: The missingness of description does not depend on `'n_ingredients'`** 

Below, we have another overlayed histogram which compares the distribution of `'n_ingredients'` when `'description'` is NaN and when it is not.

<iframe src="assets/missing_desc_ingred.html" width=800 height=600 frameBorder=0></iframe> 

For our test statistic, we again chose to use the absolute difference in means as `'n_ingredients'` is a numerical variable. Running 1000 simulations of a permutation test by shuffling the `'description'` columns, we plotted the distribution of the test statistics here: 

<iframe src="assets/empirical_plot_ingred.html" width=800 height=600 frameBorder=0></iframe> 

In the distribution above, the red line indicates the observed test statistic of our original dataseet. From our simulations, we found that the p value is 0.01 – which is reflected on the distribution as there are little to no values to the right of the observed statistic.

Therefore, using a significance level of 0.05, we **reject** the null hypothesis – meaning that the missingness in the `'description'` column **is** dependent on `'n_ingredients'` **(MAR)**.

---
## Hypothesis Testing

### Hypothesis Testing

Our projects investigates the relationship between the number of ingredients (#) and amount of calories from the recipes. During our bivariate analysis, we observed an increase in the amount of calories as the size of the shopping cart increased. For our hypothesis testing, we will test whether or not this observation will also be reflected in the results from the test.

First, we will define each of the following:

**Null Hypothesis**: the values in the distribution of the amount of calories recorded when `'shopping_cart'` is `'near_empty'` and `'overflowing'` 
comes from the same population, and the observed differences in our samples are due to random chance


**Alternative Hypothesis**: the values in the distribution of the amount of calories recorded when `'shopping_cart'` is `'near_empty'` is smaller than when `'shopping_cart'` is `'overflowing'`. The observed difference in our samples cannot be explained by random chance alone.

**Test Statistic**: difference in group means: mean calories of recipes that are considered `'overflowing'`−mean weight of non-smokers' babies. We use difference and not the absolute difference since our alternative hypothesis indicates direction.

**Significance Level**: we will use a significance level of 0.05 for our conclusion to ensure accuracy.

The graph below displays the empirical distribution of our test statistic in 1000 different permutations, under the null hypothesis stated above. 

<iframe src="assets/empirical_plot_ingred.html" width=800 height=600 frameBorder=0></iframe> 

From the graph above, we can see that the observed statistic is most likely not coincidental, as there were no cases in the 1000 simulation that resulted a test statistic equal to or more extreme than our observed statistic. From the simulation, we also found the p-value to be 0.0, which is less than our significane level of 0.05 for this hypothesis test. Therefore, we can reject the null hypothesis: the results of the test strongly suggest that the observed differences in our samples are not due to random chance, and that 

From the test conducted above, we conclude that our observed data in the dataset shows strong evidence against our null hypothesis that the difference in distribution of calorie preference in the two years are merely coincidental. Hence, we reject that there isn’t an increase in people’s preference to food with higher calories over the decade from 2008 to 2018. Although we cannot conclude the reason for this trend, the changes in the preference is highly likely a real trend over the last decade.















