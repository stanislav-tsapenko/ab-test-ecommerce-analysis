# A/B Test for E-commerce Platform

This project's Jupyter Notebook (`ab_test_ecommerce_analysis.ipynb`) calculates the statistical significance of key metrics from an A/B test on an e-commerce platform. The results are prepared for further visualization in Tableau.

---

## Project Goals
The main objective is to analyze the statistical significance of the differences between two test groups (A and B) for the following key metrics:

- **add_payment_info / session**  
- **add_shipping_info / session**  
- **begin_checkout / session**  
- **new_accounts / session**

The analysis uses a **Z-test for proportions** to determine if the observed differences are statistically significant (**p-value < 0.05**).

---

## Methodology

1. **Data Retrieval**  
   - The notebook connects to **Google BigQuery** using the `google-cloud-bigquery` library to fetch data from the `data-analytics-mate` project.

2. **SQL Query**  
   - A complex SQL query joins multiple tables (`ab_test`, `session`, `session_params`, `order`, `event_params`, `account_session`) to prepare the dataset.

3. **Data Cleaning**  
   - Data is loaded into a **Pandas DataFrame**.  
   - Performed cleaning steps: date format conversion, trimming text fields, and converting text to lowercase.

4. **Statistical Analysis**  
   - A loop iterates through test cases and metrics to calculate conversion rates for groups **A** and **B**.  
   - **Z-Test for proportions** (`statsmodels.stats.proportion.proportions_ztest`) is applied to compute Z-statistics and p-values.

5. **Results**  
   - Final results are returned as a dictionary, including:  
     - metric name  
     - conversion rates for groups A and B  
     - Z-statistic  
     - p-value  
     - significance flag (True if **p-value < 0.05**)  
   - The structured output is ready for further visualization in **Tableau**.
   ## Results

Z-test for proportions, group A vs group B, α = 0.05. Four tests, four metrics each
(16 comparisons). Conversion = events per session. Difference is B − A in percentage
points (pp).

| Test | Metric | Rate A | Rate B | Diff (pp) | p-value | Significant (p < 0.05) | After Bonferroni (p < 0.0031) |
|---|---|---|---|---|---|---|---|
| 1 | add_payment_info | 4.38% | 4.93% | +0.55 | 0.000087 | ✅ B higher | ✅ |
| 1 | add_shipping_info | 6.69% | 7.13% | +0.44 | 0.0092 | ✅ B higher | no |
| 1 | begin_checkout | 8.34% | 8.90% | +0.56 | 0.0029 | ✅ B higher | ✅ |
| 1 | new_account | 8.43% | 8.15% | −0.28 | 0.1229 | no | no |
| 2 | add_payment_info | 4.63% | 4.79% | +0.16 | 0.2146 | no | no |
| 2 | add_shipping_info | 6.87% | 6.99% | +0.12 | 0.4780 | no | no |
| 2 | begin_checkout | 8.42% | 8.58% | +0.16 | 0.3406 | no | no |
| 2 | new_account | 8.23% | 8.33% | +0.10 | 0.5560 | no | no |
| 3 | add_payment_info | 5.17% | 5.25% | +0.08 | 0.5201 | no | no |
| 3 | add_shipping_info | 7.56% | 7.37% | −0.19 | 0.1574 | no | no |
| 3 | begin_checkout | 13.61% | 13.15% | −0.46 | 0.0120 | ✅ B lower | no |
| 3 | new_account | 8.36% | 8.27% | −0.09 | 0.5199 | no | no |
| 4 | add_payment_info | 3.55% | 3.42% | −0.13 | 0.1162 | no | no |
| 4 | add_shipping_info | 4.88% | 4.71% | −0.17 | 0.0741 | no | no |
| 4 | begin_checkout | 11.95% | 11.67% | −0.28 | 0.0459 | ✅ B lower | no |
| 4 | new_account | 8.55% | 8.26% | −0.29 | 0.0175 | ✅ B lower | no |

## Conclusion

- **Test 1:** the tested change improved add_payment_info (+0.55 pp),
  add_shipping_info (+0.44 pp) and begin_checkout (+0.56 pp).
- **Tests 3 and 4:** begin_checkout went down (−0.46 pp and −0.28 pp);
  in test 4 new_account also went down (−0.29 pp).
- **Test 2:** no significant differences.

**Limitations:** 16 comparisons at α = 0.05 can produce about 0.8 false positives
by chance. With a Bonferroni correction (α ≈ 0.0031) only add_payment_info and
begin_checkout in test 1 remain significant.

**Recommendation:** the change tested in test 1 is a candidate for rollout. The changes
tested in tests 3 and 4 should be re-tested before any decision. Test 2 showed no effect.

## Data note

Data comes from BigQuery (project `data-analytics-mate`, a course dataset). There is no
public access, so the results are saved in the notebook and shown above.

---

## Tools and Libraries

- **Python**  
- **Jupyter Notebook**  
- **Google BigQuery Client**  
- **Pandas**  
- **statsmodels.stats.proportion**  
- **SQL**

---
