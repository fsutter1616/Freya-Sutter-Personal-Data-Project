# Freya-Sutter-Personal-Data-Project

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

The most extreme ZIP code had a ratio of 431. This means that for every one grocery store, there are 431 fast-food restaurants. I think that this is not a small imbalance. It represents a completely different food environment from the median ZIP code. These extreme cases match the lightest/reddest areas on the ratio map and the highest bars in the top 10 ratio chart. Overall, using the five-number summary and the IQR method helped me understand how the distribution is right skewed and why certain areas show up so dramatically in the visualizations.
