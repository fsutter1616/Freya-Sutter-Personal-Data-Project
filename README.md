# Freya-Sutter-Personal-Data-Project

## Motivation

The question I set out to answer in this project was how evenly different types of food sources are distributed across New York City, and whether certain neighborhoods face significantly worse food access than others. New York is known for having both dense commercial districts and areas with limited food access, which made me curious about how fast-food restaurants, farmers markets, and full-service grocery stores vary across ZIP codes. My main guiding question became: How imbalanced is food access across NYC ZIP codes, and which neighborhoods experience the highest concentration of fast-food restaurants relative to grocery stores and fresh food options? I chose this topic because discussions about food deserts and grocery access appear frequently in public policy conversations, especially recently with the election of NYC mayor Zohran Mamdani, whose campaign was centered around the cost of living and groceries. 

## Data Sources

To explore this question, I used three publicly available datasets from NYC Open Data and the New York State Department of Agriculture and Markets.

### 1. NYC Restaurant Inspection Results - NYC Open Data

This dataset includes over 135,000 rows of restaurant inspection records from the NYC Department of Health and Mental Hygiene (DOHMH) and is updated frequently. The full dataset contains all restaurant types, but for this project I cleaned it so that it only includes fast-food restaurants. I identified fast-food establishments by filtering based on the DBA Name  fields and isolating chain brands and known quick-service restaurants rather than full-service restaurants.

### 2. NYC Farmers Markets - NYC Open Data

This dataset lists farmers markets and food box locations across the five boroughs and is updated anually. Each row represents a single market, with ZIP codes, location details, and operational information. 

### 3. NYS Retail Food Stores (Agriculture & Markets) - NY Open Data

This dataset contains all licensed retail food stores in New York State, updated anually. I filtered it to include only full-service grocery stores in NYC ZIP codes by selecting entries with Establishment Type = “A” and Operation Type = “Store,” and then removing delis, bodegas, convenience stores, dollar stores, and other businesses that are not true supermarkets. 

## Data Process

I began with the restaurant inspection dataset, which includes more than 135,000 rows of restaurant inspection records. I first standardized the ZIP code column by padding values to five digits using str_pad(). Because this dataset contains all types of restaurants, I had to isolate fast-food locations. Using the “DBA Name” column, I filtered for chain brands and quick-service restaurants, excluding full-service dining. These are the names I included:

"McDonald", "Burger King", "Wendy", "Shake Shack", "Five Guys", "White Castle", "Checkers", "Smashburger", "Kfc", "Popeyes", "Crown Fried", "Kennedy Fried" "Chick", "Wingstop", "Taco Bell", "Chipotle", "Qdoba", "Dos Toros", "Domino", "Pizza Hut","99 Cent", "99¢", "Two Bros", "2 Bros", "Dunkin", "Krispy Kreme", "Baskin Robbins", "Carve "Ben & Jerry", "Cold Stone", "Pinkberry", "Haagen Dazs", "Dairy Queen", "Crumbl", "Starbucks"

After filtering, I used count(zipcode) to calculate the number of fast-food restaurants in each ZIP code and saved this as my fast-food dataset.

Next, I processed the farmers market dataset, which includes location and descriptive information for markets across the city. I standardized the ZIP code column, then counted the number of farmers markets per ZIP code. Unlike the fast-food data, many ZIP codes had no markets, so I used a full_join() when combining datasets later to ensure that ZIP codes with zero markets remained included.

For the full-service grocery store dataset, I again standardized ZIP codes and filtered for entries located in New York City. Because this dataset includes many types of food-related stores, I filtered it to identify supermarkets by selecting rows where Establishment Type was “A” and Operation Type was “Store.” I created a combined name field to help filter out delis, bodegas, convenience stores, pharmacies, dollar stores, and similar businesses by using keyword matching with str_detect(). After these exclusions, I counted the number of full-service grocery stores in each ZIP code. To identify full-service grocery stores, I filtered the Retail Food Stores dataset using keyword matching. I kept rows where the store’s combined name included supermarket indicators such as:

“whole foods,” “trader joe,” “aldi,” “costco,” “walmart,” “target,” “stop & shop,” “shoprite,” “kroger,” “bj’s,” “key food,” “food bazaar,” “ctown,” “c town,” “associated,” “food universe,” “pioneer,” “western beef,” “fine fare,” “met food,” “metfresh,” “food dynasty,” “sea town,” “supermarket,” “market,” “grocery,” or “foods.” 

I then removed any stores whose names contained terms associated with small shops or non-supermarkets, including:

“deli,” “bodega,” “cigar,” “smoke,” “pharmacy,” “dollar,” “99,” “convenience,” “gas,” “mart,” “mini,” “station,” “express,” “quick,” “corner,” or “halal.” 

This left me with a dataset that reflects actual full-service grocery stores rather than delis or convenience stores.

I then calculated the Fast Food : Grocery Ratio by dividing the number of fast-food restaurants by the number of grocery stores in each ZIP code, using pmax(num_groceries, 1) to avoid dividing by zero while still preserving the interpretation of extreme imbalance.

For mapping, I downloaded NYC ZIP Code Tabulation Area (ZCTA) polygons using the tigris::zctas() function and joined my dataset to these geographic shapes using left_join(). This allowed me to create choropleth maps for each variable: fast-food counts, farmers market counts, and the fast-food-to-grocery ratio.

## Visualizations

For this project, most of my visualizations are maps because the data is tied to ZIP codes, and being able to look at a map makes the differences much clearer. 

The first map I made shows the total number of fast-food restaurants in each NYC ZIP code. Midtown Manhattan has extremely high counts with some ZIP codes containing more than 400 fast-food restaraunts. Several neighborhoods in Brooklyn, the Bronx, and Staten Island also have high amounts, while other areas have much fewer fast-food restaurants.

<img src="Fastfoodmap.png" width="600">

I also created a map of farmers markets to compare access to fresh food. This map looks completely different from the fast-food one. Many ZIP codes have zero farmers markets, and only a few have more than three. This demonstrates how unevenly farmers markets are distributed compared to the density of fast-food restaurants.

<img src="Farmersmarketmap.png" width="600">

To make the differences easier to connect to specific ZIP codes, I also created a bar chart of the top 10 ZIP codes with the highest fast-food counts. ZIP codes such as 10001 and 10036 have more than 400 restaurants each, which is a huge number considering how small a ZIP code area can be. This chart helped me identify the most dense fast-food ZIP codes.

<img src="Fastfoodbarchart.png" width="600">

After cleaning the retail food store data, I also created a Fast Food to Grocery Ratio map. This is my main visualization for this project. Instead of focusing on raw numbers, this map shows how many fast-food restaurants exist for every grocery store in each ZIP code. Some neighborhoods have ratios above 200, and a few are above 300 or even 400. These neighborhoods are the ones shown in red/lighter and represent the most extreme food imbalance (based on my data and cleaning process).

<img src="Ratiomap.png" width="600">

I then made a bar chart of the top 10 ZIP codes with the highest ratios. This chart gives a simple way to see which neighborhoods face the biggest gap in food access. ZIP codes such as 10314, 11234, and 10019 are the most extreme ones as access to full-service grocery stores is very limited compared to the number of fast-food restaurants.

<img src="Ratiobarchart.png" width="600">

Overall, these visuals show both the overall distribution of food sources in NYC and the specific neighborhoods where food access is the most uneven.

## Analysis

To go beyond the visual analysis, I also used Tukey’s Five-Number Summary and the IQR rule to analyze the fast-food to grocery ratio. This ratio tells me how many fast-food restaurants exist for each grocery store in a ZIP code. I chose to use Tukey’s Five-Number Summary and the IQR rule because these techniques make it easier to understand the shape of the distribution and identify ZIP codes that differ substantially from the rest of the city. The summary was:

- Minimum: 1  
- First Quartile: 28  
- Median: 59.67  
- Mean: 76.29  
- Third Quartile: 103  
- Maximum: 431  

These values show that most ZIP codes fall between about 30 and 100, but a few ZIP codes are far above this range. When I used the IQR method to calculate outliers, several ZIP codes exceeded the upper cutoff. These outliers are visible in my boxplot.

<img src="Ratioboxplot.png" width="600">

The most extreme ZIP code had a ratio of 431. This means that for every one grocery store, there are 431 fast-food restaurants. I think that this is not a small imbalance. It represents a completely different food environment from the median ZIP code. These extreme cases match the lightest/reddest areas on the ratio map and the highest bars in the top 10 ratio chart. Using the five-number summary and the IQR method helped me understand how the distribution is right skewed and why certain areas show up so dramatically in the visualizations.

## Descriptions of Code and Materials

All of the raw data used in this project is included in the repository in CSV form, including the restaurant inspection dataset, the NYC farmers markets dataset, and the statewide retail food stores dataset. I also uploaded the cleaned fast-food dataset that I created from the restaurant inspection data. All processing and visualizations were done in my R notebook titled PDP.ipynb, which contains the code used to clean the data, filter it to NYC ZIP codes, identify full-service grocery stores, and create the maps, bar charts, and summary statistics used in the project.
