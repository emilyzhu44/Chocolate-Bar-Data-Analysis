# Chocolate Bar Analysis

## Background Info

- Collected data from Kaggle  
- Original data source is from "Flavors of Cacao"  
- 2530 observations -> 2487 after dropping NaN values  

## Cleaning Data

```python
import pandas as pd
import altair as alt

# Load dataset
df = pd.read_csv("https://raw.githubusercontent.com/Emilyz44/data301finalproject/main/chocolate.csv")
df.head()
```

```python
#cleaning ingredients list and creating a new columnb for number_of_ingredients
df = df.dropna(subset=["ingredients"])  # Drop rows with missing ingredients

df["ingredients"] = df["ingredients"].str.replace("-", ",")
df["ingredients"] = df["ingredients"].str.replace(" ", "")
df["ingredients"] = df["ingredients"].str.split(",")
df["number_of_ingredients"] = df["ingredients"].str[0]
df["ingredients"] = df["ingredients"].str[1:]

#turning ingredients list to dummy variables
ingred_dummies = pd.get_dummies(df["ingredients"].explode()).groupby(level=0).sum()

#used merge to add the dummies df to the orginal (by index)
df = df.merge(ingred_dummies, right_index=True, left_index=True)
df.head()
```

```python
#renaming ingredients from ingredient abbreviation to actual name
mapping = {
    "B": "beans",
    "S": "sugar",
    "S*": "sweetener_other_than_white_cane_or_beet_sugar",
    "C": "cocoa_butter",
    "V": "vanilla",
    "L": "lecithin",
    "Sa": "salt"
}
```

```python
#applying map to ingredients variable
df_modified = df["ingredients"].map(lambda x: [mapping[item] for item in x])
df["ingredients"] = df_modified
```

```python
df.head()
```
![df head()](https://github.com/user-attachments/assets/1f6f1d49-b30a-482a-b910-b90f4ac8f97b)

