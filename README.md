## Cosmetics Product Intelligence & Market Strategy
An end-to-end data and analytics project evaluating 1,400+ cosmetic products. This project transitions from raw data validation in SQL to advanced exploratory data analysis (EDA) in Python, concluding with executive-level strategy visualizations.

## Project Overview
This repository focuses on identifying "Value Creators" in the beauty industry. By analyzing the relationship between price, chemical ingredients, and user ratings, the project identifies which brands provide the best ROI for consumers and which carry the highest "Expectation Gap."
## Technical Stack
Database: SQLite (Data validation & Performance Gap Analysis)

Language: Python 3.x

Libraries: Pandas, NumPy, Matplotlib, Seaborn

Documentation: SQL Schema Analysis & Strategic Business Report (PDFs)

## Key Analytical Phases
1. SQL Data Validation & Aggregation
1. Pricing Strategy Analysis
📌 Insight (Detailed)

The dataset reveals that the cosmetics market is heavily dominated by mid-range products, with a balanced presence of budget and luxury segments.

The mid-range segment has the highest product concentration, indicating strong competition.
Luxury products exist but do not dominate the catalog.
Budget products maintain a steady presence, suggesting accessibility is important.

👉 Business Interpretation:
Brands are strategically positioning themselves in the mid-price segment to maximize reach and competitiveness, rather than focusing solely on premium pricing.

🧾 SQL Query
SELECT
price_segment,
COUNT(*) AS product_count
FROM cosmetics_clean
GROUP BY price_segment;
## ⭐ 2. Customer Satisfaction & Rating Behavior
📌 Insight (Detailed)

Customer ratings are consistently high across the dataset, with an average above 4.0.

This suggests overall strong product satisfaction
However, when comparing price segments:
Mid-range products often match or outperform luxury products
Budget products also maintain competitive ratings

👉 Business Interpretation:
There is no strong correlation between price and customer satisfaction. Customers prioritize effectiveness and value, not just brand prestige.

🧾 SQL Queries

Average rating:

SELECT ROUND(AVG(rating), 2) AS average_rating
FROM cosmetics_clean;

Price vs rating:

SELECT
price_segment,
ROUND(AVG(rating), 2) AS avg_rating,
COUNT(*) AS product_count
FROM cosmetics_clean
GROUP BY price_segment
ORDER BY avg_rating DESC;
## 🏷️ 3. Brand Performance & Market Positioning
📌 Insight (Detailed)

The dataset shows that a few brands dominate in terms of product count, but:

High product volume does not guarantee higher ratings
Some smaller brands compete effectively with fewer products

👉 Business Interpretation:
Success in the cosmetics industry depends more on product quality and consistency than sheer catalog size.

🧾 SQL Query
SELECT
brand,
COUNT(*) AS product_count
FROM cosmetics_clean
GROUP BY brand
ORDER BY product_count DESC;
## 🧴 4. Product Inclusivity (Skin-Type Coverage)
📌 Insight (Detailed)

Products supporting multiple skin types (e.g., dry, oily, sensitive):

Tend to have slightly higher prices
Maintain strong ratings
Represent a large portion of the dataset

👉 Business Interpretation:
Inclusivity is a key product differentiation strategy. Brands are investing in products that cater to broader audiences, increasing both value perception and market reach.

🧾 SQL Queries

Feature engineering:

UPDATE cosmetics_clean
SET skin_type_coverage = normal + dry + oily + sensitive;

Distribution:

SELECT
skin_type_coverage,
COUNT(*) AS product_count
FROM cosmetics_clean
GROUP BY skin_type_coverage
ORDER BY skin_type_coverage DESC;
## 💎 5. Value-for-Money Analysis
📌 Insight (Detailed)

By calculating a price-efficiency metric (rating/price):

The best value products are mostly mid-range or affordable
Luxury products rarely dominate in value rankings

👉 Business Interpretation:
Customers can achieve high satisfaction at lower cost, making the market highly competitive and reducing the advantage of premium pricing.

🧾 SQL Query
SELECT
brand,
product_name,
price,
rating,
ROUND(rating / price, 4) AS price_efficiency
FROM cosmetics_clean
WHERE price > 0
ORDER BY price_efficiency DESC
LIMIT 15;
## ⚠️ 6. Brand Consistency & Risk Analysis
📌 Insight (Detailed)

Brand consistency is measured using rating spread:

Brands with low variation deliver consistent quality
Brands with high variation indicate inconsistent performance

👉 Business Interpretation:
Customers are more likely to trust brands that provide consistent product experiences, not just occasional high-performing products.

🧾 SQL Query
SELECT
brand,
ROUND(AVG(rating), 2) AS avg_rating,
ROUND(MAX(rating) - MIN(rating), 2) AS rating_spread,
COUNT(*) AS product_count
FROM cosmetics_clean
GROUP BY brand
HAVING product_count >= 10
ORDER BY rating_spread ASC;
## 🛍️ 7. Consumer Recommendation System
📌 Insight (Detailed)

A “safe-buy” strategy identifies products that balance:

High rating (≥ 4.3)
Affordable price (20–40)
Broad usability (multi skin types)

👉 Business Interpretation:
Consumers prefer products that deliver high performance without premium pricing, especially when suitable for diverse skin types.

🧾 SQL Query
SELECT
brand,
product_name,
price,
rating,
skin_type_coverage
FROM cosmetics_clean
WHERE rating >= 4.3
AND price BETWEEN 20 AND 40
AND skin_type_coverage >= 3
ORDER BY rating DESC;
## 📊 8. Market Share & Competition
📌 Insight (Detailed)

Market share analysis shows:

A few brands contribute a large percentage of total products
The rest of the market is fragmented

👉 Business Interpretation:
The industry reflects moderate market concentration, where dominant brands coexist with niche competitors.

🧾 SQL Query
SELECT
brand,
COUNT(*) AS product_count,
ROUND(
COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (),
2
) AS market_share_percent
FROM cosmetics_clean
GROUP BY brand
ORDER BY market_share_percent DESC;
## 🧠 9. Advanced Analytics (Window Functions)
📌 Insight (Detailed)

Advanced SQL reveals:

Top-performing products within each brand
Products that outperform or underperform brand averages

👉 Business Interpretation:
Individual products can significantly influence brand perception and customer trust.

🧾 SQL Queries

Top 3 per brand:

WITH ranked_products AS (
SELECT
brand,
product_name,
rating,
ROW_NUMBER() OVER (
PARTITION BY brand
ORDER BY rating DESC
) AS rn
FROM cosmetics_clean
)
SELECT *
FROM ranked_products
WHERE rn <= 3;

Comparison with brand average:

SELECT
brand,
product_name,
rating,
ROUND(AVG(rating) OVER (PARTITION BY brand), 2) AS brand_avg_rating,
ROUND(rating - AVG(rating) OVER (PARTITION BY brand), 2) AS diff_from_avg
FROM cosmetics_clean;
## 2A Python Exploratory Data Analysis (EDA)
The analysis.ipynb notebook dives into the distribution of the data:

Descriptive Statistics: Analyzing price volatility and rating skews.

Correlation Mapping: Understanding the link between price points and consumer satisfaction.

Ingredient Analysis: Examining product compositions across different labels (Moisturizers, Cleansers, etc.).
2B DATA VISUALIZATION
1 Which brands create real value after adjusting for risk?
![Chart](images/chart1.png) 
This chart shows how each brand’s value contributes positively or negatively once risk is accounted for. It clearly separates brands that genuinely add value from those whose risk erodes perceived value. This format is commonly used in executive finance and strategy reviews, not student analytics.

2 Which brands are high-value but also high-risk, and which are stable performers?
![Chart](images/chart2.png)
This chart shows how each brand’s value contributes positively or negatively once risk is accounted for. It clearly separates brands that genuinely add value from those whose risk erodes perceived value. This format is commonly used in executive finance and strategy reviews, not student analytics.

3  Which products should be promoted, maintained, or discontinued?
![Chart](images/chart3.png)
This chart visually shows which products should be promoted, maintained, or removed from the portfolio.

4  What type of personality does each brand have based on price and customer satisfaction?
![Chart](images/chart4.png)

This quadrant assigns each brand a clear personality based on how expensive and how loved it is.

5 Which products exceed customer expectations and which ones disappoint?
![Chart](images/chart5.png)
This calculates how much better or worse a product performed compared to what customers expect at its price level.
This chart shows which products pleasantly surprise customers (above zero) and which disappoint them (below zero).

6  Which products are safest to recommend to customers?
![Chart](images/chart6.png)
This graph visually highlights products that are safest to recommend to customers.

7 Which products deliver the highest value relative to their price?
![Chart](images/chart7.png)
This gradient chart reveals hidden high-value products using color intensity.

8 Which low-priced products provide unexpectedly high value?
![Chart](images/chart8.png)
This graph shows where increasing price stops improving customer satisfaction.

9 At what price range does increasing price stop improving customer satisfaction?
![Chart](images/chart9.png)
This graph compares customer happiness across cheap, mid, and expensive products.

10  How does customer satisfaction compare across cheap, mid-range, and expensive products?
![Chart](images/chart9.png)
STRAGETICS  BUSINESS INSIGHTS 
The project concludes with high-level visualizations designed for stakeholders:

Brand Value Impact: A risk-adjusted view of brand equity (Value vs. Risk).

Expectation Gap Map: A quadrant analysis identifying "Surprise" vs. "Disappointment" products.

The "Safe Buy Zone": A visual recommendation engine for high-probability consumer satisfaction.

Price Sweet Spot: Identifying the exact price range where rating quality peaks before diminishing returns set in.

FILE STRUCTURE
cosmetics.csv: The raw dataset containing product ingredients and skin-type flags.

SQL.pdf: Comprehensive documentation of all SQL queries and data validation steps.

analysis.ipynb: Python environment for data processing and visualization.

ANALYSISS.pdf: Final executive summary and strategic charts.
4. 1. The "Luxury Paradox" & Diminishing Returns
The data reveals that price is not a linear predictor of quality.

Insight: While prestige brands (e.g., La Mer, SK-II) command prices over $150, their average consumer ratings often parity with mid-tier brands.

Business Impact: There is a "Price Sweet Spot" (identified in your analysis between $30 and $90) where product satisfaction is maximized. Beyond this point, consumers become hyper-critical, leading to a higher "Expectation Gap."

2. Brand Value Impact (Value vs. Risk)
By calculating Net Value Impact (Rating performance minus Price-based Risk), we can categorize brands into two strategic groups:

Value Creators: Brands like Sephora Collection or Fenty Beauty that consistently provide high ratings at accessible price points. These brands have high "Brand Equity."

Value Destroyers: Brands with high Rating Volatility. If a brand has a high average rating but high variance (Standard Deviation), it indicates a "hit or miss" product line, which erodes long-term customer loyalty.

3. The "Expectation Gap" Analysis
This is a critical metric for retail strategy.

Surprise Winners: Products that are priced low but have "prestige-level" ratings. These are the best candidates for organic viral marketing and high "Subscribe & Save" retention.

The Disappointment Zone: High-priced products with average or below-average ratings. From a business perspective, these products require a formulation pivot or a price correction to avoid damaging the parent brand's reputation.

4. Market Segmentation & Skin-Type Inclusion
The SQL audit of 1,473 products showed varying levels of "Universal Compatibility."

Insight: Brands that formulate for all five skin types (Combination, Dry, Normal, Oily, Sensitive) tend to have more stable rating distributions.

Opportunity: There is a market gap for "Sensitive-Specific" luxury items, as many high-priced products contain heavy fragrances or active ingredients that lower their compatibility scores in that specific segment.

5. The "Safe Buy Zone" (Recommendation Engine)
Your "Safe Buy Zone" visual identifies a cluster of products that represent the lowest risk for a first-time buyer.

## Strategic Application: This data can be used to build a "Smart Cart" recommendation algorithm. By filtering for products with a Value Score (Rating/Price) above 0.8, retailers can increase customer lifetime value (LTV) by ensuring the first purchase is a guaranteed success.
Key Findings
Value vs. Price: Higher price does not always correlate with higher ratings; the "Price Sweet Spot" analysis revealed significant value in mid-tier brands.

Brand Volatility: Some prestige brands show high rating variance, indicating inconsistent product quality across their line.
