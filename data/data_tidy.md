Data Tidying: Salty Salamanders
================
Georgia Lattig
04/29/24

## Load Packages and Data

``` r
op16 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2016_revised_format_QAQC.csv")
op17 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2017_revised_format_QAQC.csv")
op18 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2018_revised_format_QAQC.csv")
op19 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2019_revised_format_QAQC.csv")
op20 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2020_revised_format_QAQC.csv")
op21 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2021_revised_format_sals_only_QAQC.csv")
op22 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2022_revised_format.csv")
op23 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2023.csv")
op24 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2024.csv")
storm24 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_data - 2024 post-storm salinities.csv")
```

## Data Tidying and Merging
