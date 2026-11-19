<div align="center">
  <img src="assets/banner.png" alt="Student performance and statistical modeling banner" width="900">

# Student Performance & Persistence Analytics

**A reproducible R study connecting academic support, engagement, and exam outcomes.**

[![R](https://img.shields.io/badge/R-Statistical%20Analysis-276DC3?logo=r&logoColor=white)](https://www.r-project.org/)
[![R Markdown](https://img.shields.io/badge/R%20Markdown-Report-75AADB?logo=rmarkdown&logoColor=white)](https://rmarkdown.rstudio.com/)
[![tidyverse](https://img.shields.io/badge/tidyverse-dplyr%20%7C%20ggplot2-1A162D?logo=r&logoColor=white)](https://www.tidyverse.org/)
[![caret](https://img.shields.io/badge/caret-Train%20%26%20Split-E34F26)](https://topepo.github.io/caret/)
[![rpart](https://img.shields.io/badge/rpart-Regression%20Tree-5C2D91)](https://cran.r-project.org/package=rpart)
[![fitdistrplus](https://img.shields.io/badge/fitdistrplus-Distribution%20Fitting-FF7F0E)](https://cran.r-project.org/package=fitdistrplus)
[![MASS](https://img.shields.io/badge/MASS-Statistical%20Methods-2E8B57)](https://cran.r-project.org/package=MASS)
[![car](https://img.shields.io/badge/car-Regression%20Tools-17BECF)](https://cran.r-project.org/package=car)
</div>

## Overview & Research Objective

This project examines whether academic habits and support systems are associated with stronger student persistence and exam performance. The analysis considers parental involvement, access to resources, teacher quality, peer influence, tutoring sessions, motivation, attendance, study hours, extracurricular participation, and school context.

The central question is whether students who receive stronger academic support demonstrate more sustained effort and better outcomes. Persistence is represented by a composite `Persistence_Index`, while exam performance is measured by `Exam_Score` and derived `Score_Improvement`.

## Key Highlights & Dataset Summary

| Attribute | Project detail |
|---|---|
| Dataset | `Student Performance Factors.csv` |
| Dimensions | **6,607 observations × 20 variables** |
| Primary outcome | `Exam_Score` |
| Derived outcomes | `Score_Improvement`, `Persistence_Index` |
| Persistence formula | Mean of `-1` to `1` scaled `Hours_Studied`, `Attendance`, and `Score_Improvement` |
| Analysis tools | R, R Markdown, tidyverse, `caret`, `MASS`, `car`, `corrplot`, `nortest`, `fitdistrplus`, `rpart`, `rpart.plot` |

### Core deliverables

| File | Purpose |
|---|---|
| [`analysis.R`](analysis.R) | Script-based end-to-end pipeline with preparation, inference, and predictive modelling |
| [`analysis.Rmd`](analysis.Rmd) | Narrative R Markdown report with embedded code, tables, and figures |
| [`Student Performance Factors.csv`](Student%20Performance%20Factors.csv) | Source observations used by both analysis entry points |

## Analytical Pipeline

| Phase | What is performed |
|---|---|
| **Preparation & encoding** | Scan both `NA` values and blank strings, trim character blanks, convert blanks to `NA`, and apply complete-case filtering. Validate category labels before mapping. |
| **Ordinal transformation** | Convert ordered factors such as `Parental_Involvement`, `Access_to_Resources`, `Motivation_Level`, `Teacher_Quality`, `Family_Income`, `Peer_Influence`, `Distance_from_Home`, and `Parental_Education_Level` to numeric levels. |
| **Binary transformation** | Encode `Extracurricular_Activities`, `Internet_Access`, `Learning_Disabilities`, `Gender`, and `School_Type` as `0/1` variables. |
| **Feature engineering** | Compute `Score_Improvement = Exam_Score - Previous_Scores`, scale three components to `[-1, 1]`, and average them into `Persistence_Index`. |
| **Distribution analysis** | Fit Normal and Gamma models, compare Tutoring Sessions with a Poisson distribution, and fit Extracurricular Activities as Bernoulli/Binary; use histograms, Q-Q plots, and goodness-of-fit visuals. |
| **Hypothesis testing** | Run one-way ANOVA across candidate predictors, Welch's two-sample t-test, a two-proportion test, Levene's test, F-test/`var.test`, and Pearson correlation tests. Tukey HSD is the standard post-hoc ANOVA follow-up, although no `TukeyHSD()` call is currently present in the scripts. |
| **Regression modelling** | Fit a support-factor Multiple Linear Regression through `lm()`, inspect residual diagnostics, calculate Cook's distance, and check multicollinearity with VIF. |
| **Model alternatives** | Train an ANOVA Decision Tree with `rpart(method = "anova")` on the same 80/20 train-test split as regression and compare test RMSE and R-squared. |
| **Classifier scope** | The README's requested model scope includes Naive Bayes, but the current scripts contain no `naiveBayes()`/`e1071` implementation. They should not be represented as executed results. |

## Repository Files

```text
student-performance-factors/
├── assets/
│   └── banner.png
├── Student Performance Factors.csv
├── analysis.R
├── analysis.Rmd
├── README.md
└── .gitignore
```

## How to Run

### 1. Install R and RStudio

Use R 4.0 or newer and RStudio, if available. Open R or RStudio from the repository directory so the relative CSV path resolves correctly.

### 2. Install the required packages

Both scripts automatically check and attempt to install missing dependencies. The core package set is:

```r
install.packages(c(
  "tidyverse", "MASS", "car", "corrplot", "nortest",
  "caret", "fitdistrplus", "rpart", "rpart.plot",
  "knitr", "rmarkdown"
))
```

### 3. Execute the script

From RStudio, open `analysis.R` and run it with **Source**. Alternatively, from the project directory:

```powershell
Rscript analysis.R
```

### 4. Render the narrative report

In RStudio, open `analysis.Rmd` and use **Knit**. To render from R:

```r
rmarkdown::render("analysis.Rmd", output_format = "html_document")
```

The HTML output is written as [`analysis.html`](analysis.html).
