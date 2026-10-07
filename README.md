# CS 171 Final: Snow Crab Dashboard

An interactive R Shiny dashboard and statistical analysis of 40+ years of Bering Sea snow crab survey data (1975–2018).

Final project for **CS 171: Fundamentals of R** at UH Hilo.

<!-- screenshot: dashboard storyboard page -->

## What it does

The project asks how snow crab **haul** and **bottom depth** have changed over time across ~18,000 survey observations.

- **Normality checks:** histograms, QQ plots, and density plots for each numeric variable.
- **Kruskal–Wallis tests:** compare haul and depth across years, since the data are not normally distributed.
- **Regression:** fit to the raw data, then to yearly medians, then to medians with the 1979 outlier removed.
- **Non-linear model:** a quadratic fit to median haul over time.

### Results

| Model | Adjusted R² | Notes |
| --- | --- | --- |
| Haul ~ depth (raw data) | 0.14 | ρ = 0.42, p < .001, but explains little of the variation |
| Median depth ~ year | 0.12 | p = .013 |
| Median haul ~ year | 0.26 | ρ = 0.65, p < .001 |
| Median haul ~ year (no 1979) | 0.58 | the 1979 outlier was dragging the fit down |
| Median haul ~ year², quadratic (no 1979) | **0.67** | best fit; hints that hauls may be trending down |

## Running it

Install the R packages once:

```r
install.packages(c(
  "flexdashboard", "shiny", "plotly", "dplyr", "ggplot2", "ggthemes", "ggpubr",
  "rcompanion", "multcompView", "psych", "rstatix", "patchwork", "agricolae",
  "RColorBrewer", "car", "rmarkdown"
))
```

Then, from the repo root:

```r
# Interactive dashboard (opens in a browser; needs a live R session because it uses Shiny)
rmarkdown::run("Final_Dashboard.Rmd")

# Written analysis → CS_171_final.html
rmarkdown::render("CS_171_final.Rmd")
```

In RStudio you can also open either `.Rmd` and click **Run Document** or **Knit**. A pre-rendered `CS_171_final.html` is already included.

## Dashboard pages

| Page | What's on it |
| --- | --- |
| Data Exploration Process | Storyboard walking through the question, tests, and results |
| Normality | Histogram, QQ plot, and density plot for any numeric variable |
| Regression | Scatter plot + fit for any pair of variables you choose |
| Regression Pt 2 | The median-based models, with and without 1979 |
| Density Plots Through the Years | Haul distribution for a selected year |
| Something Extra | Haul heat map of the Bering Sea (static image; see below) |

## Files

| File | Purpose |
| --- | --- |
| `Final_Dashboard.Rmd` | The flexdashboard / Shiny app |
| `CS_171_final.Rmd` | Written analysis answering the exam prompts (stats, visuals, data management) |
| `CS_171_final.html` | Rendered version of the analysis |
| `mfsnowcrab.csv` | The dataset |
| `*.PNG` | Images embedded in the dashboard (map, map code, regression output, data frame) |

## Data

[Snow Crab dataset on Kaggle](https://www.kaggle.com/datasets/mattop/snowcrab), originally from the Resource Assessment and Conservation Engineering Division (RACE) of NOAA's Alaska Fisheries Science Center. Columns include `year`, `latitude`, `longitude`, `sex`, `bottom_depth`, `surface_temperature`, `bottom_temperature`, `haul`, and `cpue`.

The haul map on the "Something Extra" page was built with a free trial of [Stadia Maps](https://stadiamaps.com/), which needs an API key. That's why the dashboard shows a screenshot of it rather than generating it live.
