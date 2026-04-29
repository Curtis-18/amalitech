# Project Brief: The "Last Mile" Logistics Auditor

**Client:** Veridi Logistics (Global E-Commerce Aggregator)
**Deliverable:** Public Dashboard, Code Notebook & Insight Presentation

## A. Executive Summary

An audit of 96,478 delivered Olist orders reveals that approximately 8.1% of orders arrived after the estimated delivery date, with 4.4% classified as "Super Late" (more than 5 days). Late deliveries are strongly correlated with lower review scores, ontime orders average while super late orders collapse to 1.79/5, this directly validates the CEO's hypothesis that broken delivery promises are driving negative reviews.
The problem is concentrated in Northern states (AL,MA,PI,CE), with Alagoas showing a 23.8% late rate compared to Sao Paulo's 5.9%, this suggests a regional logistics infrastructure not a nationwide failure.
Monthly trend analysis shows the late rate has remained stable throughout 2017-2018, confirming this has been a log-standing problem rather than a recent deterioration.

## Project Links

- **Link to Notebook:** [https://github.com/Curtis-18/amalitech/blob/master/data-engineering/Logistics-auditor/Logistics.ipynb]
- **Link to Dashboard:**[https://datastudio.google.com/reporting/1d710ced-72f5-4ab8-9895-676bade706bf]
- **Link to Presentation:** [https://www.overleaf.com/read/ffjtxvbnmzqj#75a902]


## C. Technical Explanation

**Data Cleaning:** The main challenge was a 1-to-many join issue where 547 orders had multiple reviews, inflating the merged dataset from 99,441 to 99,992 rows. This was fixed by deduplicating the reviews table before merging, keeping only the most recent review per order (sorted by review_answer_timestamp). Eight orders in the delivered subset had null delivery dates and were excluded from delay calculations. Canceled and unavailable orders were excluded from all delivery analysis by filtering to order_status == 'delivered' only.

**Candidate's Choice — Monthly Trend Analysis:** I added a month-by-month chart of late delivery percentage from 2016 to 2018. This matters to the business because the diagnosis changes depending on the trend: a rising trend means the logistics network is failing to scale with order volume (a capacity problem requiring infrastructure investment), while a flat trend means the problem is structural and regional (requiring targeted courier partnerships in underserved states). The data shows a broadly flat ~8% late rate throughout the period, pointing to the latter — a persistent structural gap in northeastern Brazil rather than a scaling crisis.
