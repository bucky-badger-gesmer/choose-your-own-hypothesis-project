# Detection Method and Insolation Flux in Exoplanets

## Project Scaffold

| Element                               | Your Plan                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Topic / Question**                  | Does discovery method influence the insolation flux of detected planets? If so, are our catalogs of potentially Earth-like worlds shaped by search technique rather than true planetary distribution?                                                                                                                                                                                                                                                                     |
| **Hypothesis**                        | **H₀:** Discovery method has no influence on insolation flux. The mean (or median) insolation flux of planets found by the Transit method is equal to that of planets found by the Radial Velocity method. Any observed difference is due to random chance.<br><br>**H₁:** Discovery method does influence insolation flux. The mean (or median) insolation flux of planets found by the Transit method differs from that of planets found by the Radial Velocity method. |
| **Outcome / Metric / Test Statistic** | pl_insol (Insolation Flux [Earth Flux]); difference in means (or medians) between Transit and Radial Velocity groups                                                                                                                                                                                                                                                                                                                                                      |
| **Units of Analysis**                 | Individual exoplanet entries from the NASA Exoplanet Archive                                                                                                                                                                                                                                                                                                                                                                                                              |
| **Data Source(s)**                    | [NASA Exoplanet Archive](https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=PS) — PS_2026.04.26_08.19.28.csv                                                                                                                                                                                                                                                                                                                        |
| **Why this data works**               | The dataset contains 39,803 exoplanet entries with discovery method, insolation flux, and equilibrium temperature — enough observations for both Transit (35,713) and Radial Velocity (2,885) groups to support resampling-based inference                                                                                                                                                                                                                                |
| **Uncertainty Metric**                | Bootstrap confidence interval for the difference in mean/median insolation flux between the two discovery methods                                                                                                                                                                                                                                                                                                                                                         |
| **Null Hypothesis**                   | The type of detection technique astronomers use does not change the amount of starlight received by the planets they find. Any difference in insolation flux between Transit and Radial Velocity planets is due to random chance.                                                                                                                                                                                                                                         |

## 1. Research Question

What are you investigating, and why does it matter?

## 2. Hypothesis

State your null and alternative hypotheses clearly and succinctly.

## 3. Data Description

Describe your data source(s):

- Where it comes from (URL, API, dataset name)
- What each observation represents (unit of analysis)
- Number of observations and key variables
- Any filtering, cleaning, or transformation steps

## 4. Methods

Summarize how you analyzed the data:

- The test statistic for your permutation test
- How you simulated or resampled under the null hypothesis
- The metric(s) for which you created bootstrap confidence intervals
- Why the CLT does not apply to at least one metric

## 5. Results

Present your main findings:

- Key summary statistics and visualizations
- Observed test statistic and p-value (if applicable)
- Bootstrap confidence intervals for relevant metrics

## 6. Uncertainty Estimation

Discuss your resampling results:

- How many resamples you used
- What the bootstrap or randomization distributions looked like
- How you interpret the interval estimates

## 7. Limitations

Briefly note any limitations in data, assumptions, or methods, including sources of bias or missing data.

## 8. References

List all datasets, tools, libraries, or papers you cited.

---

**Reminder:** Your README should be clear enough that someone unfamiliar with your work could understand what you studied, how you analyzed it, and what you found.
