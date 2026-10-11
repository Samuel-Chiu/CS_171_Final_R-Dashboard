# Bering Sea Snow Crab Survey: R Analysis and Shiny Dashboard

An interactive R Shiny dashboard and statistical analysis of 44 years (1975–2018) of NOAA eastern Bering Sea bottom-trawl survey records for snow crab, about 18,000 observations.

Final project for **CS 171: Fundamentals of R** at UH Hilo.

## Questions

How do two survey variables, `haul` and `bottom_depth`, change across survey years, and how strongly are they related?

## Methods

| Step | Approach |
| --- | --- |
| Distribution checks | Histograms, QQ plots, density plots, and Shapiro–Wilk tests by year |
| Differences between years | Kruskal–Wallis tests (the data are not normal), with `agricolae::kruskal` letter groupings |
| Relationships | Spearman correlation and linear regression on the raw data, then on yearly medians |
| Outlier check | Refit after removing 1979, which sits far from the other yearly medians |
| Non-linear fit | Quadratic model of median `haul` over time |

## Results

| Model | Adjusted R² | Notes |
| --- | --- | --- |
| `haul ~ bottom_depth` (raw) | 0.14 | ρ = 0.42, p < .001, but explains little variation |
| Median `bottom_depth ~ year` | 0.12 | p = .013 |
| Median `haul ~ year` | 0.26 | ρ = 0.65, p < .001 |
| Median `haul ~ year`, 1979 removed | 0.58 | 1979 was pulling the fit down |
| Median `haul ~ year + year²`, 1979 removed | **0.67** | best fit |

### A note on the `haul` variable

In this dataset `haul` is the **haul number**, the ID of an individual trawl tow within a survey year (values run from 1 to 334). It is not a measure of how much crab was caught; that is `cpue` (catch per unit effort). The trends above therefore describe how haul numbering shifted across survey years, not changes in snow crab abundance. I didn't recognize this when I did the project, and I've left the original analysis unchanged as a record of the coursework. Repeating it with `cpue` as the response is the natural next step.

## Running it

Install the R packages once:

```r
install.packages(c(
  "flexdashboard", "shiny", "plotly", "dplyr", "ggplot2", "ggthemes", "ggpubr",
  "rcompanion", "multcompView", "psych", "rstatix", "patchwork", "agricolae",
  "RColorBrewer", "car", "rmarkdown"
))
```

Then, from the repository root:

```r
rmarkdown::run("scripts/dashboard.Rmd")      # interactive dashboard (needs a live R session)
rmarkdown::render("scripts/analysis.Rmd")    # written analysis -> scripts/analysis.html
```

In RStudio you can also open either `.Rmd` and click **Run Document** or **Knit**. A pre-rendered `scripts/analysis.html` is included; download it and open it in a browser to read the analysis without R.

## Dashboard pages

| Page | Contents |
| --- | --- |
| Data Exploration Process | Storyboard walking through the question, tests, and results |
| Normality | Histogram, QQ plot, and density plot for any numeric variable |
| Regression | Scatter plot and fit for any pair of variables |
| Regression Pt 2 | The median-based models, with and without 1979 |
| Density Plots Through the Years | `haul` distribution for a selected year |
| Something Extra | Map of survey haul locations (static image, see below) |

## Repository layout

```
scripts/
  analysis.Rmd     written analysis answering the exam prompts
  analysis.html    rendered analysis
  dashboard.Rmd    flexdashboard + Shiny app
data/              mfsnowcrab.csv
figures/           images embedded in the dashboard
```

## Data

[Snow Crab dataset on Kaggle](https://www.kaggle.com/datasets/mattop/snowcrab), derived from the eastern Bering Sea bottom-trawl survey run by the Resource Assessment and Conservation Engineering (RACE) Division of NOAA's Alaska Fisheries Science Center. Columns: `id`, `latitude`, `longitude`, `year`, `name`, `sex`, `bottom_depth` (m), `surface_temperature` and `bottom_temperature` (°C), `haul`, and `cpue`.

The map on the "Something Extra" page was made with a free trial of [Stadia Maps](https://stadiamaps.com/), which requires an API key, so the dashboard shows a saved image instead of generating it live.