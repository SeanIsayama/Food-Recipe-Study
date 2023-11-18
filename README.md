# Food-Recipe-Study
**Authors**: Hikaru Isayama, Julia Jung

---

## Introduction and Question Identification

This data science project investigates the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted since 2008 on food.com. With the continuing concerns about health in the US, having a more accurate understanding of calorie intake helps maintain a healthy weight, as consuming too many calories can lead to weight gain and associated health issues like obesity, diabetes, and heart problems. This project investigates a new indicator (number of ingredients) to see whether or not this value has any impact on the amount of calories in the recipe.

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

1. The 'nutrition' column of the first dataset. This column contains information about the recipe in the following format: <br><br>"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"<br><br>To allow the calories (#) to be more accessible, we decided to create a new column called calories in the merged dataset, which we will reference instead of the entire 'nutrition' column

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

[IMAGE HERE]

Above is a histogram that shows the distribution of the number of ingredients per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 5-15 ingredient range, with most of the outliers more than the majority – leading to a right- skewed distribution. 

[IMAGE HERE]

Above is another histogram that shows the distribution of the number of calories per recipe in our dataset. In this histogram, we can see that the majority of recipes are within the 0-500 calorie range, with again most of the outliers being on the greater end – leading to a right-skewed distribution. 

### Bivariate Analysis

As our study investigate the relationship between two quantitative variables (number of ingredients, amount of calories), we decided to plot our data on a scatter plot. 

[IMAGE HERE]

Above is a scatter plot with the number of ingredients on the x-axis, and the amount of calories (#) on the y-axis. We can observe that the scatter plot does not show a strong correlation between the two variables. In other words, the number of ingredients does not appear to have a significant effect on the amount of calories. 

### Interesting Aggregates

---
## Assessment of Missingness

### NMAR Analysis

In our dataset before we filtered, the `'description'` column could possibly be NMAR. This could be due to the reviewer’s interest in investing time or pride about the recipe, which is not measured in our dataset. For example, reviewers who have a lot of time at their hands may write descriptions for the recipes they submit, while those who do not may not. Similarly, reviewers who are more proud about the recipe they submit may be more motivated to write a description, while those who are not may omit it entirely. However, these values are not recorded in our dataset, therefore making the `'description'` column a NMAR.

We can change the `'description'` column from NMAR to MAR if we include variables that measure the free time reviewers have, or how proud they are of the recipe. For example, if there was a rating scale of how proud they are about the recipe they submit, the missingness of the `'description'`column could be influenced by the low values measured on this new variable. 

### Missingness Dependency

---










