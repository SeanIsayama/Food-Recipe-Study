# Food-Recipe-Study
**Authors**: Hikaru Isayama, Julia Jung

---

## Introduction and Question Identification

This data science project investigates the relationship between the number of ingredients (#) and amount of calories from the recipes and reviews posted since 2008 on food.com. With the continuing concerns about health in the US, having a more accurate understanding of calorie intake helps maintain a healthy weight, as consuming too many calories can lead to weight gain and associated health issues like obesity, diabetes, and heart problems. This project investigates a new indicator (number of ingredients) to see whether or not this value has any impact on the amount of calories in the recipe.

### Datasets used (food.com)

The first dataset we used on this project contains recipes posted on food.com since 2008, containing 83782 rows representing each recipe posted, with each row consisting these 12 columns of descriptions: 

|Column	                 |Description|
|---                     |---        |
|`'name'	`            |Recipe name|
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

1. The 'nutrition' column of the first dataset. This column contains information about the recipe in the following format: 

"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"  

To allow the calories (#) to be more accessible, we decided to create a new column called calories in the merged dataset, which we will reference instead of the entire 'nutrition' column

2. The  ‘n_ingredients’ column of the first dataset. This column contains the value for the number of ingredients in the recipe, which we use to compare with the calorie value of that recipe.

---