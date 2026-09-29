# Model selection in multilevel models

A short, reproducible tutorial in R on how to choose — and how to judge — models with random effects.
Prepared for the *Multilevel Models* seminar at TU Dortmund University, Department of Statistics (February 2026).

**Slides:** https://elegantway25.github.io/mlm-model-selection/ ← *(enable GitHub Pages for this repo; see "Rendering" below)*

## What it covers

Selecting a mixed-effects model is harder than selecting a regression: you choose the fixed effects *and* the
random-effects structure, variance parameters can sit on the boundary of the parameter space (a variance of
exactly zero), and different criteria answer different questions — prediction is not the same goal as inference
on fixed effects.

The tutorial works through two simulated settings and compares the tools side by side:

| Setting | Data | Question |
|---|---|---|
| Part 1 | Clustered data, 40 groups × 30 observations | Random intercepts vs random slopes; which fixed effects? |
| Part 2 | Crossed random effects, 60 subjects × 40 items | Which random-effects structure, and how much does the choice matter? |

Methods demonstrated:

- **Likelihood-ratio tests** for nested models, fitted with ML rather than REML when fixed effects differ
- **AIC and BIC** across a candidate set
- **Conditional AIC** (`cAIC4`), aimed at conditional prediction
- **Marginal and conditional R²** (Nakagawa, via `performance`)
- **Model averaging** with Akaike weights (`MuMIn`)

## Reporting checklist

- Define the candidate set in advance and justify it substantively
- Use ML for comparisons involving fixed effects; watch for boundary issues in variance components
- Report AIC / BIC / cAIC together with diagnostics and R², not a single number
- When several models are similarly supported, consider model averaging instead of picking one

## Running it

```r
install.packages(c("rmarkdown", "xaringan", "lme4", "lmerTest",
                   "dplyr", "ggplot2", "performance", "MuMIn", "cAIC4"))
rmarkdown::render("model-selection-multilevel.Rmd")
```

All data are simulated with fixed seeds (`set.seed(1)`, `set.seed(2)`), so every number in the slides is
reproducible. `sessionInfo()` is printed on the last slide.

## Background

The tutorial draws on three papers, each answering a different question about mixed models. Full references
are below; the summaries are my own.

- **How well does a fitted model describe the data?** Cantoni, Jacot and Ghisletta review measures of explained
  variation for linear mixed models and show that most R²-type measures are *relative*: they depend on the null
  model one compares against, so "variation explained by the covariates" and "variation explained by covariates
  given the clustering" are different quantities. In their simulations, conditional AIC performed best for model
  comparison, particularly under REML.
- **Which model should be chosen from a set of candidates?** Müller, Scealy and Welsh survey the field and sort
  the methods into four families: information criteria (including marginal and conditional AIC), shrinkage
  approaches, fence procedures, and Bayesian methods. Their conclusion is that no method wins universally — the
  right choice depends on the goal and on how large the candidate set is.
- **How much does the choice of random-effects structure distort inference?** Martínez-Huertas and co-authors
  simulate crossed designs and compare bottom-up and top-down selection strategies against model averaging.
  Correct selection depended far more on the design — the number of items in particular — than on the strategy,
  and standard errors of within-subject effects were the most vulnerable to getting it wrong.



## Licence

Code: MIT (see `LICENSE`). Slide text: © Danaia Burtseva, 2026.
