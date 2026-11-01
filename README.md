# Statistical Analysis and Predictive Modeling using R

Statistical analysis, hypothesis testing and multiple linear regression in R to predict **penguin body mass** from the Palmer Penguins dataset. The project covers data cleaning, exploratory analysis, ANOVA and correlation tests, assumption checks, 5-fold cross-validation, test-set evaluation, model diagnostics and model comparison.

> Internship project | Data Analytics | Author: **Divya Ahire**

---

## Table of Contents

- [Project Overview](#project-overview)
- [Research Question and Objectives](#research-question-and-objectives)
- [Dataset](#dataset)
- [Tools and Technologies](#tools-and-technologies)
- [Project Structure](#project-structure)
- [How to Run](#how-to-run)
- [Methodology](#methodology)
- [Results](#results)
- [Model Diagnostics](#model-diagnostics)
- [Model Comparison](#model-comparison)
- [Conclusion](#conclusion)
- [Skills Demonstrated](#skills-demonstrated)
- [References](#references)

---

## Project Overview

This project combines statistical inference with predictive modeling. After cleaning and exploring the data, it tests whether body mass differs between species, measures the link between flipper length and body mass, and builds a regression model that is validated on unseen data and checked against its statistical assumptions.

## Research Question and Objectives

**Can we predict a penguin's body mass using its flipper length, bill measurements, species, sex and island?**

- Understand the dataset through exploratory statistical analysis
- Test whether mean body mass differs between species (ANOVA and Tukey HSD)
- Test the relationship between flipper length and body mass (Pearson correlation)
- Check normality, equal variance, linearity and multicollinearity
- Build a multiple linear regression model on an 80/20 train/test split
- Evaluate with 5-fold cross-validation, test-set RMSE, MAE and R-squared
- Run diagnostics and compare improved models

## Dataset

[Palmer Penguins](https://allisonhorst.github.io/palmerpenguins/) (`palmerpenguins` R package)

| Item | Detail |
|---|---|
| Original size | 344 rows x 8 columns |
| After cleaning | 333 complete rows (11 incomplete rows removed, 0 duplicates) |
| Target | `body_mass_g` |
| Predictors | `flipper_length_mm`, `bill_length_mm`, `bill_depth_mm`, `species`, `sex`, `island` |

## Tools and Technologies

| Tool | Purpose |
|---|---|
| R, RStudio | Analysis environment |
| tidyverse (dplyr, tidyr, ggplot2) | Data preparation and plots |
| caret | Data splitting, cross-validation, evaluation metrics |
| car | Levene's test, variance inflation factors |
| lmtest | Breusch-Pagan test |
| randomForest | Model comparison |

## Project Structure

```
.
├── penguins_analysis.R       # Full R script (cleaning, tests, modeling)
├── Screenshots/              # RStudio output screenshots
├── Report/                   # Final project report (Word/PDF)
└── README.md
```

## How to Run

```r
install.packages(c("palmerpenguins", "tidyverse", "caret", "car",
                   "lmtest", "ggplot2", "randomForest"))
```

```r
source("penguins_analysis.R")
```

A fixed seed (`set.seed(123)`) is used so the train/test split and cross-validation folds are reproducible.

## Methodology

1. **Data cleaning:** checked missing values and duplicates, selected model columns, applied `drop_na()`, converted categorical variables to factors
2. **Exploratory analysis:** summary statistics, species-wise comparison, histogram, box plot and scatter plot
3. **Hypothesis testing:** one-way ANOVA with Tukey HSD, and Pearson correlation test
4. **Assumption checks:** Shapiro-Wilk, Levene's test, correlation matrix, linearity plot, with Welch's ANOVA and Kruskal-Wallis as robustness checks
5. **Splitting:** 80% training (268 rows) and 20% testing (65 rows) with `createDataPartition()`
6. **Modeling:** multiple linear regression with `lm()`
7. **Validation:** 5-fold cross-validation on the training data only
8. **Evaluation:** RMSE, MAE and R-squared on the held-out test data
9. **Diagnostics and improvement:** residual plots, VIF, Breusch-Pagan test, reduced model, interaction model and Random Forest

## Results

### Hypothesis tests

| Test | Result | Conclusion |
|---|---|---|
| ANOVA (body mass by species) | F(2, 330) = 341.9, p < 2e-16 | Reject H0: mean body mass differs by species |
| Tukey HSD | Gentoo differs from Adelie (+1,386 g) and Chinstrap (+1,359 g); Adelie vs Chinstrap p = 0.916 | Gentoo is much heavier; Adelie and Chinstrap are similar |
| Pearson correlation | r = 0.873, 95% CI 0.845-0.896, p < 2.2e-16 | Strong positive relationship |

Levene's test (p = 0.0064) showed unequal variances between species, so the ANOVA result was confirmed with Welch's ANOVA and Kruskal-Wallis (both p < 2.2e-16).

### Model performance

| Evaluation | RMSE (g) | MAE (g) | R-squared |
|---|---|---|---|
| 5-fold cross-validation | 295.04 | 236.36 | 0.8645 |
| Test dataset (65 penguins) | 294.25 | 238.31 | 0.8835 |

The close agreement between cross-validation and test results indicates the model generalizes well. Species, sex, flipper length, bill length and bill depth were significant predictors; island was not.

## Model Diagnostics

- **Residuals vs fitted / Q-Q / scale-location:** no clear curvature, approximately normal residuals, roughly constant spread
- **Breusch-Pagan test:** p = 0.097, constant variance not rejected
- **Multicollinearity:** moderate; species has an adjusted GVIF of 2.81 and the other adjusted values range from 1.39 to 2.68
- **Influence:** largest Cook's distance is 0.036, so no point is individually influential

## Model Comparison

| Model | CV RMSE | Test RMSE | Test R-squared |
|---|---|---|---|
| M1 Full model (6 predictors) | 295.0 | 294.3 | 0.883 |
| M2 Without island | 291.1 | 294.8 | 0.883 |
| M3 Without island and bill length | 294.2 | 286.8 | 0.889 |
| M4 With flipper x species interaction | 292.0 | 289.5 | 0.887 |
| M5 Random Forest | 302.9 | 295.3 | 0.896 |

All models perform within about 10 g RMSE of each other. **M2 (without island)** is recommended: it is simpler and has the best cross-validated RMSE. The Random Forest offered no advantage.

## Conclusion

Penguin body mass can be predicted to within roughly 240 g on average (MAE) using a multiple linear regression model that explains about 88% of the variation in unseen data. Species and sex produce the largest shifts in mass, flipper length is the strongest numerical predictor, and island adds no useful information.

## Skills Demonstrated

- Data cleaning and preprocessing in R
- Hypothesis testing (ANOVA, Tukey HSD, Pearson correlation)
- Checking and reporting statistical assumptions
- Multiple linear regression and interpretation of coefficients
- Cross-validation, train/test evaluation and model comparison
- Regression diagnostics (residuals, VIF, Breusch-Pagan, Cook's distance)

## References

- [Palmer Penguins dataset](https://allisonhorst.github.io/palmerpenguins/)
- [palmerpenguins R package](https://cran.r-project.org/package=palmerpenguins)
- [caret](https://topepo.github.io/caret/) | [car](https://cran.r-project.org/package=car) | [lmtest](https://cran.r-project.org/package=lmtest) | [tidyverse](https://www.tidyverse.org/)
