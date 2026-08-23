# Revenue Optimization & Cancellation Risk Modelling for Hotel Operations

A business analytics project analyzing hotel booking data to optimize revenue and predict cancellation risk. It combines exploratory data analysis with financial impact quantification to identify actionable insights for revenue management.

### Stack

- **Language(s):** R (RMarkdown)
- **Framework / runtime:** R with tidyverse ecosystem
- **Notable libraries:** `tidyverse`, `dplyr`, `ggplot2`, `ggridges`, `scales`, `patchwork` — all used for data manipulation and publication-quality business visualizations

## How it's organized

```         
Data/                         Raw and processed hotel booking datasets
  Raw_Data/                   Source data
  Processed_Data/             Cleaned and feature-engineered data

Data Cleaning/
  data_preprocessing.Rmd      Load, deduplicate, feature engineering
                              Creates transaction-level metrics (revenue, lead time, room match)

Exploratory_Data_Analysis/
  00_exploratory_analysis.Rmd         Daily booking patterns, data quality checks
  01_hotel_revenue_analysis.Rmd       ADR distribution, total revenue, revenue impact of cancellations
  02_cancellations_analysis.Rmd       Lead time segments, cancellation rates, revenue loss by segment
  03_customer_segmentation.Rmd        Market segment analysis, deposit type impact, customer type behavior

Hotel Analytics.Rproj         R project configuration
```

**How it fits together:** Data flows from raw hotel booking CSV (from tidytuesday) through preprocessing to calculate transaction-level metrics (total revenue, lead time buckets, guest counts, room assignment flags). Each exploratory analysis then slices this cleaned dataset to answer specific business questions: revenue distribution, cancellation drivers by lead time and market segment, and customer behavior segmentation. All outputs are rendered as HTML/PDF reports with publication-quality visualizations.

## How to run it

Each R Markdown file runs independently after the data preprocessing step:

``` bash
# 1. Run preprocessing (generates Data/Processed_Data/final_hotel_bookings.csv)
Rscript Data\ Cleaning/data_preprocessing.Rmd

# 2. Run exploratory analyses (generates HTML/PDF reports)
Rscript Exploratory_Data_Analysis/00_exploratory_analysis.Rmd
Rscript Exploratory_Data_Analysis/01_hotel_revenue_analysis.Rmd
Rscript Exploratory_Data_Analysis/02_cancellations_analysis.Rmd
Rscript Exploratory_Data_Analysis/03_customer_segmentation.Rmd
```

Or within RStudio: Open the `.Rmd` files and click "Knit" to render the HTML/PDF outputs.

**Required:** Local file path in preprocessing must exist: `~/Documents/00 Working Projects/Hotel Data Analytics/Hotel Analytics/Data/Processed_Data/` (or update paths in all files).

## Try asking

- What are the lead time segments with the highest revenue loss, and how do they differ from highest-volume segments?
- How does deposit type interact with market segment to reduce cancellation risk?
- Which customer type and market segment combination has the worst cancellation exposure?

# Results

![](images/average_daily_rate_trend_plot)

![](images/patched_adr_rev_plot)

![](images/revenue_exposure_plot)

![](images/net_realized_rev_plot)

![](images/lead_time_cancel_exp_plot)
