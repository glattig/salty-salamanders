Data Tidying: Salty Salamanders
================
Georgia Lattig
04/29/24

## Load Packages and Data

``` r
op16 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2016.csv")
op17 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2017.csv")
op18 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2018.csv")
op19 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2019.csv")
op20 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2020.csv")
op21 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2021.csv")
op22 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2022.csv")
op23 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2023.csv")
op24 <- read_csv("/cloud/project/data/raw/otter_point_REFORMATTED - 2024.csv")
```

## Data Tidying and Merging

``` r
salamanders <- rbind(op16, op17, op18, op19, op20, op21, op22, op23, op24)

write_csv(salamanders, "/cloud/project/data/tidied/otter_point_salamanders.csv")

view(salamanders)
```
