# Project Brief: The "Sugar Trap" Market Gap Analysis

**Client:** Helix CPG Partners (Strategic Food & Beverage Consultancy)
**Deliverable:** Interactive Dashboard, Code Notebook & Insight Presentation

## A. Executive Summary

Analysis of 300,000 food products found that only 6% of snacks fall in the "High Protein / Low Sugar" zone, revealing a clear market gap. The biggest opportunity is in Cereals & Granola, where demand exists but most products still carry too much sugar. The top protein ingredients driving success in healthy products are Soy, Peanuts, and Whey. The recommendation is to launch a granola or cereal product with ≥14g protein and <3g sugar per 100g to capture this underserved space.

## B. Project Links

- **Link to Notebook:** [https://github.com/Curtis-18/amalitech/blob/master/data-engineering/The-Market-Gap-Analysis-main/market-gap-analysis.ipynb]
- **Link to Dashboard:**[https://datastudio.google.com/s/pI8aXj1fd5E]
- **Link to Presentation:** []

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

Key finding: Protein Bars & Supplements is the only category with a positive score (+~38), confirming it as the nutritional benchmark. All other categories 
score negative, meaning their average product does more nutritional harm than good by this measure. Notably, Fruit & Veggie Snacks scores the worst of all 
(~-40) — this is not a data error. This category in the dataset is dominated by dried fruits (raisins, dates, mango strips) which carry ~50g of sugar per 
100g, driving the score down. It does not reflect the nutritional value of fresh fruit.

This addition matters to the business because it gives the client's R&D team a product specification target: beat a score of +10 and you have a demonstrably 
healthier product than anything currently in most snack categories.

---

## 1. Business Context

**Helix CPG Partners** advises major food manufacturers on new product development. Our newest client, a global snack manufacturer, wants to launch a "Healthy Snacking" line. They believe the market is oversaturated with sugary treats, but they lack the data to prove where the specific gaps are.

They have hired us to answer one question: **"Where is the 'Blue Ocean' in the snack aisle?"**

Specifically, they are looking for product categories that are currently under-served—areas where consumer demand for health (e.g., High Protein, High Fiber) is not being met by current product offerings (which are mostly High Sugar, High Fat).

## 2. The Data

You will use the **Open Food Facts** dataset, a free, open, and massive database of food products from around the world.

- **Source:** [Open Food Facts Data](https://world.openfoodfacts.org/data)
- **Format:** CSV (Comma Separated Values)
- **Warning:** The full dataset is massive (over 3GB). You are **not** expected to process the entire file. You should filter the data early or work with a manageable subset (e.g., the first 500,000 rows or specific categories).

## 3. Tooling Requirements

You have the flexibility to choose your development environment:

- **Option A (Recommended):** Use a cloud-hosted notebook like **Google Colab**, or **Deepnote**, etc.
- **Option B:** Use a local **Jupyter Notebook** or **VS Code**.
  - _Condition:_ If you choose this, you must ensure your code is reproducible. Do not reference local file paths (e.g., `C:/Downloads/...`). Assume the dataset is in the same folder as your notebook.
- **Dashboarding:** The final output must be a **publicly accessible link** (e.g., Tableau Public, Google Looker Studio, Streamlit Cloud, or PowerBI Web).

---

## 4. User Stories & Acceptance Criteria

### Story 1: Data Ingestion & "The Clean Up"

**As a** Strategy Director,
**I want** a clean dataset that removes products with erroneous nutritional information,
**So that** my analysis is not skewed by bad data entry.

- **Acceptance Criteria:**
  - Handle missing values: Decide what to do with rows that have `null` or empty `sugars_100g`, `proteins_100g`, or `product_name`.
  - Handle outliers: Filter out biologically impossible values.
  - **Deliverable:** A cleaned Pandas DataFrame or SQL table export.

### Story 2: The Category Wrangler

**As a** Product Manager,
**I want** to group products into readable high-level categories,
**So that** I don't have to look at 10,000 unique, messy tags like `en:chocolate-chip-cookies-with-nuts`.

- **Acceptance Criteria:**
  - The `categories_tags` column is a comma-separated string (e.g., `en:snacks, en:sweet-snacks, en:biscuits`). You must parse this string.
  - Create a logic to assign a "Primary Category" to each product based on keywords.
  - Create at least 5 distinct high-level buckets.

### Story 3: The "Nutrient Matrix" Visualization

**As a** Marketing Lead,
**I want** to see a Scatter Plot comparing Sugar (X-axis) vs. Protein (Y-axis) for different categories,
**So that** I can visually spot where the products are clustered.

- **Acceptance Criteria:**
  - Create a dashboard (PowerBI, Tableau, Streamlit, or Python-based charts) displaying this relationship.
  - Allow the user to filter the chart by the "High Level Categories" you created in Story 2.
  - **Key Visual:** Identify the "Empty Quadrant" (e.g., High Protein + Low Sugar).

### Story 4: The Recommendation

**As a** Client,
**I want** a clear text recommendation on what product we should build,
**So that** I can take this to the R&D team.

- **Acceptance Criteria:**
  - On the dashboard, include a "Key Insight" box.
  - Complete this sentence: _"Based on the data, the biggest market opportunity is in [Category Name], specifically targeting products with [X]g of protein and less than [Y]g of sugar."_

---

## 5. Bonus User Story: The "Hidden Gem"

**As a** Health Conscious Consumer,
**I want** to know which specific ingredients are driving the high protein content in the "good" products,
**So that** I can replicate this in our new recipe.

- **Acceptance Criteria:**
  - Analyze the `ingredients_text` column for products in your "High Protein" cluster.
  - Extract and list the Top 3 most common protein sources (e.g., "Whey", "Peanuts", "Soy").

---

## 6. The "Candidate's Choice" Challenge

**As a** Creative Analyst,
**I want** to add one additional feature or analysis to this project that I believe provides massive value,
**So that** I can show off my business acumen.

- **Instructions:**
  - Add one more chart, filter, or metric that wasn't asked for.
  - Explain **why** you added it.
  - **There is no wrong answer, but you must justify your choice.**

---

## 7. Submission Guidelines

Please edit this `README.md` file in your forked repository to include the following three sections at the top:

### A. The Executive Summary

- A 3-5 sentence summary of your findings.

### B. Project Links

- **Link to Notebook:** (e.g., Google Colab, etc.). _Ensure sharing permissions are set to "Anyone with the link can view"._
- **Link to Dashboard:** (e.g., Tableau Public / Power BI Web, etc.).
- **Link to Presentation:** A link to a short slide deck (PDF, PPT) AND (Optional) a 2-minute video walkthrough (YouTube) explaining your results.

### C. Technical Explanation

- Briefly explain how you handled the "Data Cleaning".
- Explain your "Candidate's Choice" addition.

**Important Note on Code Submission:**

- Upload your `.ipynb` notebook file to the repo.
- **Crucial:** Also upload an **HTML or PDF export** of your notebook so we can see your charts even if GitHub fails to render the notebook code.
- Once you are ready, please fill out the [Official Submission Form Here](https://forms.cloud.microsoft/e/CeQN2mCyUr) with your links

---

## 🛑 CRITICAL: Pre-Submission Checklist

**Before you submit your form, you MUST complete this checklist.**

> ⚠️ **WARNING:** If you miss any of these items, your submission will be flagged as "Incomplete" and you will **NOT** be invited to an interview.
>
> **We do not accept "permission error" excuses. Test your links in Incognito Mode.**

### 1. Repository & Code Checks

- [x] **My GitHub Repo is Public.** (Open the link in a Private/Incognito window to verify).
- [x] **I have uploaded the `.ipynb` notebook file.**
- [x] **I have ALSO uploaded an HTML or PDF export** of the notebook.
- [x] **I have NOT uploaded the massive raw dataset.** (Use `.gitignore` or just don't commit the CSV).
- [x] **My code uses Relative Paths.**

### 2. Deliverable Checks

- [x] **My Dashboard link is publicly accessible.** (No login required).
- [ ] **My Presentation link is publicly accessible.** (Permissions set to "Anyone with the link can view").
- [x] **I have updated this `README.md` file** with my Executive Summary and technical notes.

### 3. Completeness

- [ ] I have completed **User Stories 1-4**.
- [ ] I have completed the **"Candidate's Choice"** challenge and explained it in the README.

**✅ Only when you have checked every box above, proceed to the submission form.**

---
