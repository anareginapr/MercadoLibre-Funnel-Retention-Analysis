# MercadoLibre — Funnel & Retention Analysis

### Product Analytics Case Study | SQL

Analysis of the user purchase journey to identify major funnel drop-offs and evaluate user retention across countries and monthly cohorts.

---

## 🎯 Business Problem

As part of a Product Analytics team focused on Growth & Retention, the goal of this analysis is to understand:

- Where users drop off during the purchase journey.
- How conversion varies across countries.
- How well users are retained after registration.
- Whether retention patterns differ across monthly cohorts.

The analysis covers the period from *January 1 to August 31, 2025*.

---

## 🗂️ Dataset

The project uses two tables:

### mercadolibre_funnel

Contains user events throughout the purchase journey, including:

- first_visit
- select_item
- select_promotion
- add_to_cart
- begin_checkout
- add_shipping_info
- add_payment_info
- purchase

Additional dimensions include:

- Country
- Device category
- Platform
- Product category
- Price
- Referral source
- Event date

### mercadolibre_retention

Contains user registration and activity information used to measure retention, including:

- User ID
- Signup date
- Country
- Device category
- Platform
- Days after signup
- Activity date
- Active status

---

## 🔎 Methodology

Data Schema & Exploration
Prior to building the funnel and cohort queries, initial data exploration was performed on the source tables (mercadolibre_funnel and mercadolibre_retention) to verify column data types, event sequencing, and unique user identifiers across the period 2025-01-01 to 2025-08-31.

### 1. Funnel Analysis

I built the purchase funnel using SQL CTEs, creating a separate user-level dataset for each stage.

DISTINCT user_id was used to ensure that each user was counted only once per stage.

The analysis starts with first_visit and uses LEFT JOIN to preserve users who entered the funnel even if they did not continue to later stages.

Conversion rates were calculated relative to the initial first_visit population.

### 2. Country Segmentation

The funnel was segmented by country to identify differences in conversion performance and determine where the largest drop-offs occur across markets.

### 3. Retention Analysis

Retention was calculated at:

- D7
- D14
- D21
- D28

Users were counted as retained when they were active at or beyond the corresponding day after signup.

### 4. Cohort Analysis

Users were grouped into monthly cohorts based on their first signup date.

This allowed retention behavior to be compared across users who registered during different months.

---

## 📊 Key Findings

### 1. The largest funnel drop-off occurs before Add to Cart

The overall funnel shows a significant drop between product selection and adding a product to the cart:

| Funnel Stage | Conversion from First Visit |
|---|---:|
| Select Item / Promotion | 76.90% |
| Add to Cart | 11.01% |
| Begin Checkout | 4.00% |
| Add Shipping Info | 2.42% |
| Add Payment Info | 2.09% |
| Purchase | 1.25% |

The largest drop-off occurs between *Select Item / Promotion and Add to Cart*, indicating a critical point of friction before purchase intent becomes explicit.

---

### 2. Conversion varies across countries

Final purchase conversion varies considerably by country.

| Country | Purchase Conversion |
|---|---:|
| Uruguay | 4.55% |
| Bolivia | 3.23% |
| Mexico | 2.48% |
| Peru | 1.82% |
| Argentina | 1.25% |
| Chile | 1.03% |
| Brazil | 0.68% |
| Ecuador | 0.00% |
| Colombia | 0.00% |
| Paraguay | 0.00% |

Mexico shows a higher final conversion rate than Paraguay in the analyzed dataset.

However, conversion percentages alone do not explain the reasons behind these differences or indicate the absolute size of each country's user population.

---

### 3. Retention decreases significantly over time

Retention is relatively strong at D7 but declines substantially after D14.

Across countries, D7 retention is above 79% in the analyzed results, while D28 retention falls to approximately 1.6%–3.2%.

Mexico and Peru show the highest D28 retention among the countries analyzed, at 3.1% and 3.2%, respectively.

This suggests an opportunity to investigate the factors contributing to the strong decline in longer-term engagement.

---

### 4. Retention varies across monthly cohorts

Monthly cohorts show relatively consistent early retention from January through July, followed by a much sharper decline in the August cohort.

| Cohort | D7 | D14 | D21 | D28 |
|---|---:|---:|---:|---:|
| 2025-01 | 86.2% | 56.2% | 24.1% | 3.0% |
| 2025-02 | 86.8% | 56.0% | 24.6% | 2.7% |
| 2025-03 | 87.7% | 56.8% | 26.6% | 3.0% |
| 2025-04 | 87.2% | 53.9% | 23.0% | 2.0% |
| 2025-05 | 86.0% | 54.5% | 26.2% | 3.0% |
| 2025-06 | 85.9% | 55.1% | 25.2% | 2.1% |
| 2025-07 | 86.4% | 56.4% | 25.9% | 2.7% |
| 2025-08 | 70.8% | 29.7% | 7.5% | 0.2% |

The August cohort should be interpreted cautiously because the analysis period ends on August 31, 2025. Users who registered late in the month may not have had a complete observation window to reach D28.

---

## 💡 Recommendations

### 1. Investigate the Select Item → Add to Cart drop-of

The first priority should be understanding what is causing the largest funnel drop.

Potential hypotheses could include:

- Product pricing or perceived value.
- Product information quality.
- Reviews or social proof.
- User experience or interface friction.
- Shipping or additional costs.

*Actionable Next Step:* These are hypotheses rather than conclusions and require additional data to validate. Applying a **Root Cause Analysis (RCA)** framework combined with targeted **A/B testing** will help isolate friction points regarding pricing transparency and UI mechanics at the item selection stage

### 2. Analyze conversion by traffic source

Segmenting the funnel by referral_source could help determine whether certain acquisition channels bring users with lower purchase intent or whether there is a mismatch between acquisition messaging and the product experience.

### 3. Compare device and platform performance

Analyzing device_category and platform could help identify whether the drop-off is concentrated among mobile, desktop, web, Android, or iOS users.

This could reveal potential usability or technical issues.

### 4. Analyze product categories

Comparing conversion by product_cat could reveal categories with unusually high or low performance and generate additional hypotheses around product characteristics, pricing, or purchase intent.

### 5. Investigate post-D14 retention

The strong decline after D14 suggests an opportunity to investigate strategies focused on second purchases, reactivation, and customer loyalty.

---

## ⚠️ Analytical Considerations

This analysis identifies *where* users drop off and *how* retention varies, but the available variables do not establish causal relationships.

For example, the low Add to Cart conversion does not prove that users are abandoning because of product price, reviews, interface issues, or shipping costs.

Further analysis and additional data would be required to validate these hypotheses.

Similarly, cohort comparisons should account for differences in observation windows, particularly for recent cohorts such as August 2025.

---

## 🛠️ Tools & Skills

*SQL*

- CTEs
- DISTINCT
- LEFT JOIN
- Aggregations
- CASE WHEN
- Date functions
- Conversion rate calculations

*Analytics*

- Funnel analysis
- Cohort analysis
- Retention analysis
- Segmentation
- Business problem framing
- Hypothesis generation
- Data interpretation
