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
### [Q1: Does Continent of Bean Origin Affect Rating?](./chocolate_analysis.ipynb#q1-does-the-beans-continent-of-origin-have-a-statistically-significant-impact-on-rating)
- Box plot showed lower median for "Blend", but mean differences were significant in post-hoc analysis.

- **Tukey HSD** test results:
  - "Blend" rated higher than:
  - Africa (p = 0.0257)
  - North America (p = 0.0138)
  - South America (p = 0.0037)

<div align="center">
      
**Interpretation: Higher means may be influenced by positive outliers in the "Blend" group.** 

</div>


### [Q2: Does Ingredient Type Impact Rating?](./chocolate_analysis.ipynb#q2-are-certain-characteristics-more-associated-with-chocolate-bars-of-specific-ingredients-bean-origins-and-ratings)

Bars with more complex recipes (e.g., vanilla or lecithin added) tended to have higher ratings. Not all differences were significant, but several ingredient combinations stood out in post-hoc analysis.

Tukey HSD test results:
Bars with beans, sugar, cocoa butter, and vanilla rated higher than:
beans and sugar (p < 0.0000)
beans, sugar, and cocoa butter (p < 0.0000)
beans, sugar, cocoa butter, and lecithin (p < 0.0000)


Bars with beans, sugar, cocoa butter, vanilla, and lecithin rated higher than:
beans and sugar (p = 0.0092)
beans, sugar, and cocoa butter (p < 0.0000)


Bars with beans, sugar, and lecithin rated higher than:
beans and sugar (p = 0.0396)
beans, sugar, and cocoa butter (p = 0.0118)


Bars with alternative sweeteners rated higher than:
beans, sugar, and cocoa butter (p = 0.0053)

OLS Model: Statistical significant rating when grouped by ingredient group



sum_sq
df
F
PR(>F)
ingredients
24.017872
20.0
6.858351
7.151651e-19
Residual
424.090908
2422.0
NaN
NaN

Tukey Output here
Q3: Does Number of Ingredients Affect Rating? 
Chocolate bars with 4 and 5 ingredients had a statistically greater rating than bars with 2 and 3 ingredients

OLS Model: Statistical significant rating when grouped by number of ingredients


sum_sq
df
F
PR(>F)
number_of_ingredients
4.377233
1.0
24.079483
9.853255e-07
Residual
443.731547
2441.0
NaN
NaN


Tukey Output here
Q4: Can the rating of a chocolate be predicted well using the information about its origin and ingredients?
Despite some strong training R² scores, all models underperformed in validation — suggesting overfitting and limited predictive power with available variables.

Model 
r^2 (Train)
Cross Validation Score
K-neighbor Regressor
0.16451022011857908
-0.04220942435762818
Voting Regressor
0.04453802830913156
-0.004608377846626132
Stacking Regressor
0.052100773800294764
0.021330995160120225
Ridge Regressor
0.41062779690905826
-0.031115755961708447
Decision Tree
0.8015835494060043
-0.31724584566011876

Recommendations 
Modeling Recommendations
Avoid R² alone as a performance metric; consider MAE or RMSE.
Try balancing variables through random undersampling (eg:there were significantly more South American and North American bean origins in our data and company location was heavily skewed within the US)
Check for multicollinearity between ingredient indicators.
Non-linear models (e.g., Random Forests or Neural Networks) may capture more nuance.

Business Recommendations
Ingredient Strategy:
Avoid combinations: "beans and sugar", or "beans, sugar, cocoa butter", and “beans, sugar, cocoa butter and lecithin”
Favor 4–5 ingredient formulations over other ingredient formulations (with the exception of “beans, sugar, cocoa butter and lecithin”)
Sourcing Strategy:
Consider incorporating blended origin beans or exploring origins other than Africa, North America, or South America.
Further Data Needs:
Explore variables such as price, brand reputation, or reviewer ID to improve rating prediction.

