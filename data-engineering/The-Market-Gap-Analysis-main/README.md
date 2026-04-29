# Project Brief: The "Sugar Trap" Market Gap Analysis

**Client:** Helix CPG Partners (Strategic Food & Beverage Consultancy)
**Deliverable:** Interactive Dashboard, Code Notebook & Insight Presentation

## A. Executive Summary

Analysis of 300,000 food products found that only 6% of snacks fall in the "High Protein / Low Sugar" zone, revealing a clear market gap. The biggest opportunity is in Cereals & Granola, where demand exists but most products still carry too much sugar. The top protein ingredients driving success in healthy products are Soy, Peanuts, and Whey. The recommendation is to launch a granola or cereal product with ≥14g protein and <3g sugar per 100g to capture this underserved space.

## B. Project Links

- **Link to Notebook:** [https://github.com/Curtis-18/amalitech/blob/master/data-engineering/The-Market-Gap-Analysis-main/market-gap-analysis.ipynb]
- **Link to Dashboard:**[https://datastudio.google.com/s/pI8aXj1fd5E]
- **Link to Presentation:** [https://www.overleaf.com/read/jmqkxmqyddwq#92b52d]

## Technical Explanation

### Data Cleaning

The raw dataset was loaded using pandas read_csv() with sep='\t' (Open Food Facts uses tab, not comma, as its delimiter) and restricted to 300,000 rows and 9 relevant columns to keep memory usage manageable. 

Cleaning was done in two stages:
1. Null removal — rows missing product_name, sugars_100g, or proteins_100g were dropped entirely, reducing the dataset from 300,000 to 45,475 rows. These columns are non-negotiable for the core analysis; a product without a name or nutritional data cannot be meaningfully categorised.
2. Outlier removal — a hard boundary filter was applied: all nutrient values must fall between 0g and 100g per 100g of product, which is a physical impossibility to exceed. Values outside this range are data entry errors (e.g. a sugar value    of 450g/100g). This removed a further 140 rows, leaving 45,335 clean records.
No imputation (filling in missing values with averages) was used for the core sugar and protein columns — it is better to exclude unknown values than to invent 
them, which would distort the nutritional analysis.

### Candidate's Choice: The Category Healthiness Score

A composite "Healthiness Score" was calculated for each product using the formula:

$$Score = Protein + Fiber − Sugar − (Fat × 0.5)$$

Fat is penalised at half weight because not all fat is nutritionally harmful (e.g. the healthy fats in nuts), while sugar is penalised at full weight as 
it provides no nutritional benefit. This gives R&D a single defensible number to benchmark new formulations against existing categories, rather than juggling 
four separate nutrient columns.

Key finding: Protein Bars & Supplements is the only category with a positive score (\~38), confirming it as the nutritional benchmark. All other categories 
score negative, meaning their average product does more nutritional harm than good by this measure. Notably, Fruit & Veggie Snacks scores the worst of all 
(\~-40) , this is not a data error. This category in the dataset is dominated by dried fruits (raisins, dates, mango strips) which carry ~50g of sugar per 
100g, driving the score down. It does not reflect the nutritional value of fresh fruit.

This addition matters to the business because it gives the client's R&D team a product specification target: beat a score of +10 and you have a demonstrably 
healthier product than anything currently in most snack categories.

