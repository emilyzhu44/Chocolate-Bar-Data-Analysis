## Project Overview

The Chocolate Bar Ratings dataset was created by the **Manhattan Chocolate Society** through 65+ curated tastings between 2007–2016. The group evaluated fine dark chocolate bars based on factors like cacao origin, bean type, and manufacturing process. Data was gathered from expert reviews, direct sourcing, and collaboration with chocolate makers. The goal: to explore and document the unique flavor profiles of high-quality chocolate.

**This analysis aims to uncover:**

1. Whether **bean origin**, **ingredient type**, or **ingredient count** impact chocolate ratings.
2. Whether chocolate ratings can be **predicted** using available features.
3. Business recommendations based on statistically significant findings.

**Tools & Libraries Used**

- Pandas: Data cleaning, transformation, and EDA
- Altair: Data visualization
- scikit-learn: Predictive modeling and cross-validation
- statsmodels: ANOVA and Tukey HSD testing

## Data structure overview 

The database structure as seen below consists of 1 table with a total row count of 2443.

<div align="center">
      
| Column Name                                        | Type    |
|---------------------------------------------------|---------|
| ref                                               | int64   |
| company_manufacturer                              | object  |
| company_location                                  | object  |
| review_date                                       | int64   |
| country_of_bean_origin                            | object  |
| specific_bean_origin_or_bar_name                  | object  |
| cocoa_percent                                     | float64 |
| ingredients                                       | object  |
| most_memorable_characteristics                    | object  |
| rating                                            | float64 |
| number_of_ingredients                             | int64   |
| beans, cocoa_butter, lecithin, sugar, vanilla...  | int64   |
| continent_of_bean_origin                          | object  |

</div>

## Executive Summary

1. Bean Origin:
    - Statistically significant differences in ratings were found by continent of bean origin.
    - The "Blend" group had significantly higher average ratings than Africa, North America, and South America.
  
2. Ingredient Composition:
    - Out of 209 ingredient comparisons, 8 showed statistically significant higher ratings. 
    - Bars with added vanilla, lecithin, or alternative sweeteners tended to rate higher than some of the simpler recipes, though most differences were not significant.
  
3. Ingredient Count:
    - Chocolate bars with 4 or 5 ingredients had significantly higher average ratings than those with 2 or 3 ingredients.
  
4. Predictive Modeling:
    - Predictive models built using origin and ingredient data performed poorly, especially during cross-validation.
    - Suggests that subjectivity or unobserved variables (e.g., price, branding) play a key role in ratings.
  
## Deep Dive Analysis
### Q1: Does the Continent of Bean Origin Affect Rating?

- Box plot showed lower median for "Blend", but mean differences were significant in post-hoc analysis.

**Tukey HSD test results:** 

"Blend" rated higher than:
- Africa (p = 0.0257)
- North America (p = 0.0138)
- South America (p = 0.0037)

<div align="center">
      
**Interpretation: Higher means may be influenced by positive outliers in the "Blend" group.** 

</div>

<div align="center">
      
![continent of bean origin and rating box and whiskers](https://github.com/emilyzhu44/Chocolate-Bar-Data-Analysis-/blob/main/visualization%20(1).png?raw=true)

</div>

<div align="center">

| group1        | group2        | meandiff | p-adj   | lower   | upper   | reject |
|---------------|---------------|----------|---------|---------|---------|--------|
| Africa        | Blend         | -0.1299  | 0.0257  | -0.2503 | -0.0095 | True   |
| Blend         | North America | 0.1271   | 0.0138  | 0.0163  | 0.2378  | True   |
| Blend         | South America | 0.1397   | 0.0037  | 0.0304  | 0.2491  | True   |

</div>

<div align="center">
      
##### Statistically Signifcant Tukey HSD Output

</div>

### Q2: Does the Ingredient Type Impact Rating?

- Bars with more complex recipes (e.g., vanilla or lecithin added) tended to have higher ratings. Not all differences were significant, but several ingredient combinations stood out in post-hoc analysis.

**Tukey HSD test results:**
- Bars with beans, sugar, cocoa butter, and vanilla rated higher than:
  - beans and sugar (p < 0.0000)
  - beans, sugar, and cocoa butter (p < 0.0000)
  - beans, sugar, cocoa butter, and lecithin (p < 0.0000)


- Bars with beans, sugar, cocoa butter, vanilla, and lecithin rated higher than:
  - beans and sugar (p = 0.0092)
  - beans, sugar, and cocoa butter (p < 0.0000)


- Bars with beans, sugar, and lecithin rated higher than:
  - beans and sugar (p = 0.0396)
  - beans, sugar, and cocoa butter (p = 0.0118)


- Bars with alternative sweeteners rated higher than:
  - beans, sugar, and cocoa butter (p = 0.0053)

<div align="center">
      
|               | sum_sq        | df       | F       | PR(>F)      | 
|---------------|---------------|----------|---------|---------    |
| ingredients   | 24.017872     | 20.0     |6.858351 | <code> 7.151651e-19 </code>| 
| Residual      | 424.090908    | 2422.0   | NaN     | NaN         | 

</div>

<div align="center">
      
##### OLS Model: Ingredients with Rating

</div>

<div align="center">

| group1                             | group2                                                             | meandiff | p-adj  | lower    | upper    | reject |
|-----------------------------------|---------------------------------------------------------------------|----------|--------|----------|----------|--------|
| beans, sugar                      | beans, sugar, cocoa_butter, vanilla                                 | -0.2543  | 0.0000 | -0.3920  | -0.1166  | True   |
| beans, sugar                      | beans, sugar, cocoa_butter, vanilla, lecithin                       | -0.1398  | 0.0092 | -0.2633  | -0.0162  | True   |
| beans, sugar                      | beans, sugar, lecithin                                              | -0.5420  | 0.0396 | -1.0735  | -0.0104  | True   |
| beans, sugar, cocoa_butter        | beans, sugar, cocoa_butter, vanilla                                 | -0.3034  | 0.0000 | -0.4379  | -0.1688  | True   |
| beans, sugar, cocoa_butter        | beans, sugar, cocoa_butter, vanilla, lecithin                       | -0.1889  | 0.0000 | -0.3088  | -0.0689  | True   |
| beans, sugar, cocoa_butter        | beans, sugar, lecithin                                              | -0.5910  | 0.0118 | -1.1218  | -0.0603  | True   |
| beans, sugar, cocoa_butter        | beans, sweetener_other_than_white_cane_or_beet_sugar                | -0.3189  | 0.0053 | -0.5915  | -0.0462  | True   |
| beans, sugar, cocoa_butter, lecithin | beans, sugar, cocoa_butter, vanilla                               | -0.2381  | 0.0000 | -0.3920  | -0.0843  | True   |

</div>

<div align="center">
      
##### Statistically Signifcant Tukey HSD Output

</div>


### Q3: Does the Number of Ingredients Affect Rating?

- Chocolate bars with 4 and 5 ingredients had a statistically greater rating than bars with 2 and 3 ingredients

<div align="center">
      
|               | sum_sq        | df       | F       | PR(>F)      | 
|---------------|---------------|----------|---------|---------    |
| number_of_ingredients   | 4.377233     | 1.0     |24.079483 | <code> 9.853255e-07 </code> | 
| Residual      | 443.731547    | 2441.0   | NaN     | NaN         | 

</div>

<div align="center">
      
##### OLS Model: Number of Ingredients and Rating

</div>

<div align="center">

| group1 | group2 | meandiff | p-adj  | lower   | upper   | reject |
|--------|--------|----------|--------|---------|---------|--------|
| 2      | 4      | -0.0901  | 0.0042 | -0.1612 | -0.0189 | True   |
| 2      | 5      | -0.1382  | 0.0008 | -0.2361 | -0.0402 | True   |
| 3      | 4      | -0.1409  | 0.0000 | -0.2083 | -0.0735 | True   |
| 3      | 5      | -0.1890  | 0.0000 | -0.2842 | -0.0937 | True   |

</div>

<div align="center">
      
##### Statistically Signifcant Tukey HSD Output

</div>


### Q4: Can the rating of a chocolate be predicted well using the information about its origin and ingredients?

- Despite some strong training R² scores, **all models underperformed in validation** — suggesting **overfitting** and limited predictive power with available variables.

<div align="center">
      
| Model              | R² (Train)           | Cross Validation Score     |
|--------------------|----------------------|-----------------------------|
| K-neighbor Regressor | 0.1645               | -0.0422                     |
| Voting Regressor     | 0.0445               | -0.0046                     |
| Stacking Regressor   | 0.0521               | 0.0213                      |
| Ridge Regressor      | 0.4106               | -0.0311                     |
| Decision Tree        | 0.8016               | -0.3172                     |
      
</div>

## Recommendations 

**Modeling Recommendations**

- Avoid R² alone as a performance metric; consider MAE or RMSE.
- Try balancing variables through random undersampling (eg:there were significantly more South American and North American bean origins in our data and company location was heavily skewed within the US)
- Check for multicollinearity between ingredient indicators.
- Non-linear models (e.g., Random Forests or Neural Networks) may capture more nuance.

**Business Recommendations**

- Ingredient Strategy:
  - Avoid combinations: "beans and sugar", or "beans, sugar, cocoa butter", and “beans, sugar, cocoa butter and lecithin”
  - Favor 4–5 ingredient formulations over other ingredient formulations (with the exception of “beans, sugar, cocoa butter and lecithin”)
- Sourcing Strategy:
  - Consider incorporating blended origin beans or exploring origins other than Africa, North America, or South America.
- Further Data Needs:
  - Explore variables such as price, brand reputation, or reviewer ID to improve rating prediction.

