Data Tidying: Salty Salamanders
================
Georgia Lattig
04/29/24

## Load Packages and Data

``` r
op16 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2016.csv")
op17 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2017.csv")
op18 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2018.csv")
op19 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2019.csv")
op20 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2020.csv")
op21 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2021.csv")
op22 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2022.csv")
op23 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2023.csv")
op24 <- read_csv("/Users/georgialattig/salty salamanders/data/raw/otter_point_REFORMATTED - 2024.csv")
```

## Data Tidying and Merging

``` r
op22 <- op22 %>% 
  mutate(date = format.Date(date, "%m/%d/%y"))
```

``` r
op22 <- op22 %>% 
  mutate(pit = str_remove_all(pit, "900043000"))
```

``` r
salamanders <- rbind(op16, op17, op18, op19, op20, op21, op22, op23, op24)
```

``` r
salamanders %>% 
  filter(pit != "none",
         pit != "not scanned",
         pit != "unknown") %>% 
  count(pit) %>% 
  arrange(desc(n))
```

    ## # A tibble: 288 × 2
    ##    pit        n
    ##    <chr>  <int>
    ##  1 243216    19
    ##  2 243345    16
    ##  3 243348    15
    ##  4 243261    12
    ##  5 243274    12
    ##  6 243399    12
    ##  7 243337    11
    ##  8 243403    11
    ##  9 243264    10
    ## 10 243276    10
    ## # ℹ 278 more rows

As of the end of the 2024 field season, 288 unique PIT tags have been
deployed in this population for a total of 287 PIT-tagged individuals
(one salamander was accidentally PIT-tagged twice).

``` r
salamanders %>% 
  group_by(pit) %>% 
  count(first_year)
```

    ## # A tibble: 417 × 3
    ## # Groups:   pit [292]
    ##    pit    first_year     n
    ##    <chr>  <chr>      <int>
    ##  1 243205 2019           2
    ##  2 243206 2019           4
    ##  3 243207 2019           3
    ##  4 243208 2019           2
    ##  5 243209 2022           1
    ##  6 243209 <NA>           1
    ##  7 243210 2022           1
    ##  8 243210 <NA>           1
    ##  9 243211 2019           5
    ## 10 243212 2022           2
    ## # ℹ 407 more rows

The code above reveals that there are some individuals with multiple
first_year values associated with them (either multiple color tags or
pit_year values).

Below I created a massive case_when function to recode the data as
necessary. I recoded `first_year` based on information about year of PIT
tag (these are concrete data) or based on the most frequent color tag
recorded across seasons. In cases with equal numbers of observations of
different color tags, I looked for additional information in the notes
column or else trusted the observation with highest sampling effort
(largest field crew size). Reasons are given for each decision within
the code below.

``` r
salamanders <- salamanders %>% 
  mutate(first_year = case_when(
    pit == "243212" ~ "2022", #PIT tag given 2022
    pit == "243216" ~ "2020_2021", #once recorded as pink in 2023 but all other times, across 2 seasons, red
    pit == "243218" ~ "2022", #recorded as new in 2023 but was first tagged with PIT in 2022; was NOT double-tagged
    pit == "243229" ~ "2020_2021", #once recorded as pink in 2023 but all other times, across 2 seasons, red
    pit == "243234" ~ "2022", #first tagged with PIT in 2022
    pit == "243237" ~ "2017", #yellow tag likely missed in first/only encounter in 2022, all other observations are yellow
    pit == "243244" ~ "2018", #recorded red in 2022 but accompanying notes read "kind of pink VIE" + recorded pink in 2023
    pit == "243248" ~ "2020_2021", #once recorded as pink in 2023 but all other times, across 2 seasons, red
    pit == "243262" ~ "2018", #once recorded as orange in 2023 but all other times, across nights, pink
    pit == "243273" ~ "2020_2021", #no color recorded in 2022 but multiple nights in 2023 recorded red
    pit == "243275" ~ "2022", #no color recorded when first PIT tagged 2022 nor all other times except one "yellow" in 2023 ***
    pit == "243284" ~ "2022", #first tagged with PIT in 2022
    pit == "243289" ~ "2018", #recorded pink 4 nights in 2022 and red 3 nights in 2023...
    pit == "243304" ~ "2019", #recorded 2x orange in 2022 including accompanying notes about the orange tag, 2023 "might not be pink, hard to tell"
    pit == "243316" ~ "2020_2021", #1x red in 2022, 1x orange in 2023; trusted 2022 observation due to larger field crew and "effort"
    pit == "243321" ~ "2022", #no color recorded when first PIT tagged 2022 nor other times except one "yellow" in 2023 ***
    pit == "243322" ~ "2020_2021", #once recorded as orange in 2023 but all other times, across nights, red
    pit == "243328" ~ "2020_2021", #once no color recorded in 2023 but all other times, across nights, red
    pit == "243339" ~ "2017", #once recorded red in 2023 but all other times, across nights, yellow
    pit == "243343" ~ "2022", #first tagged with PIT in 2022
    pit == "243345" ~ "2022", #first tagged with PIT in 2022, all observations no color except one orange record in 2023
    pit == "243358" ~ "2022", #first tagged with PIT in 2022
    pit == "243359" ~ "2020_2021", #once recorded orange in 2023 but all other times, across nights, red
    pit == "243360" ~ "2018", #no color recorded but notes say pink
    pit == "243361" ~ "2019", #no color recorded when first PIT tagged in 2022 nor other 2022 observations but twice recorded as orange in 2023
    pit == "243363" ~ "2020_2021", #once no color recorded in 2023 but all other times, across seasons, red
    pit == "243370" ~ "2019", #recorded orange 4x in 2022, once pink and twice red in 2023
    pit == "243374" ~ "2017", #yellow tag likely missed in first/only encounter in 2022, all other observations are yellow
    pit == "243375" ~ "2017", #recorded yellow across 3 nights in 2023, no colors recorded in all prior observations ***
    pit == "243377" ~ "2022", #UV light forgotten so no color tag but first tagged with PIT 2022
    pit == "243380" ~ "2022", #first tagged with PIT in 2022
    pit == "243382" ~ "2022", #first tagged with PIT in 2022
    pit == "243383" ~ "2022", #first tagged with PIT in 2022
    pit == "243393" ~ "2022", #first tagged with PIT in 2022
    pit == "243396" ~ "2020_2021", #once recorded pink in 2023 but all other times, across seasons, red
    pit == "243399" ~ "2022", #no color recorded when first PIT tagged in 2022 nor all other 2022 observations except 2x recorded as red in 2023 with accompanying notes "may not be red"
    pit == "243401" ~ "2020_2021", #no color recorded when first PIT tagged in 2022 and only once recorded "red" but additional notes from another night say "has color tag but couldn't read it" ***
    pit == "243402" ~ "2019", #once recorded pink in 2023 but all other times, across nights, orange
    pit == "243403" ~ "2020_2021", #once recorded pink in 2023 but all other times, across seasons, red
    pit == "244741" ~ "2018", #always recorded pink
    pit == "244752" ~ "2019", #most often recorded orange
    pit == "244766" ~ "2023", #first tagged with PIT in 2023
    pit == "244797" ~ "2020_2021", #possible that the red color tag was missed when first PIT tagged 2023
    pit == "244832" ~ "2020_2021", #possible that the red color tag was missed when first PIT tagged 2023, all other times red
    pit == "244838" ~ "2023", #first tagged with PIT in 2023
    pit == "244842" ~ "2023", #first tagged with PIT in 2023
    pit == "244854" ~ "2023", #first tagged with PIT in 2023
    pit == "244880" ~ "2023", #first tagged with PIT in 2023
    pit == "244883" ~ "2023", #first tagged with PIT in 2023
    pit == "244888" ~ "2023", #first tagged with PIT in 2023
    pit == "244898" ~ "2020_2021", #possible that the red color tag was missed when first PIT tagged 2023, all other times red
    pit == "243209" ~ "2022", #PIT tag given 2022
    pit ==  "243210" ~ "2022", #PIT tag given 2022
    pit == "243215" ~ "2018", #noted pink in 2024 when there were 9 field techs present,noted red in 2022
    pit == "243220" ~ "2020_2021", #noted pink once in 2024 but all other times red
    pit == "243223" ~ "2022", #PIT tag given 2022
    pit == "243228" ~ "2022", #PIT tag given 2022
    pit == "243232" ~ "2022", #PIT tag given 2022
    pit == "243236" ~ "2022", #PIT tag given 2022
    pit == "243241" ~ "2022", #PIT tag given 2022
    pit == "243246" ~ "2019", #always noted orange except for once when no VIE was detected
    pit == "243250" ~ "2022", #PIT tag given 2022 
    pit == "243255" ~ "2022", #PIT tag given 2022  
    pit == "243257" ~ "2022", #PIT tag given 2022 
    pit == "243258" ~ "2019", #once noted pink in 2024 but all other times orange
    pit == "243260" ~ "2018", #once noted orange in 2024 but all other times pink
    pit == "243272" ~ "2022", #PIT tag given 2022
    pit == "243279" ~ "2022", #PIT tag given 2022
    pit == "243286" ~ "2022", #PIT tag given 2022
    pit == "243302"~ "2022", #PIT tag given 2022
    pit == "243305" ~ "2022", #PIT tag given 2022
    pit == "243309" ~ "2018", #noted as both pink and red during 2024 but pink when there was a crew of 9 field techs at OP
    pit == "243311" ~ "2022", #PIT tag given 2022
    pit == "243319" ~ "2022", #PIT tag given 2022
    pit == "243320" ~ "2022", #PIT tag given 2022
    pit == "243326" ~ "2020_2021", #noted orange once in 2023 but all other times red
    pit == "243332" ~ "2022", #PIT tag given 2022
    pit == "243331" ~ "2022", #PIT tag given 2022
    pit == "243333" ~ "2022", #PIT tag given 2022
    pit == "243337" ~ "2022", #PIT tag given 2022
    pit == "243341" ~ "2022", #PIT tag given 2022
    pit == "243342" ~ "2022", #PIT tag given 2022
    pit == "243350" ~ "2022", #PIT tag given 2022
    pit == "243357" ~ "2019", #marked red once in 2024 but all other times red
    pit == "243362" ~ "2022", #PIT tag given 2022
    pit == "243368" ~ "2022", #PIT tag given 2022
    pit == "243379" ~ "2022", #PIT tag given 2022
    pit == "243397" ~ "2022", #PIT tag given 2022
    pit == "243400" ~ "2022", #PIT tag given 2022
    pit == "243404" ~ "2022", #PIT tag given 2022
    pit == "244720" ~ "2023", #PIT tag given 2023
    pit == "244732" ~ "2023", #PIT tag given 2023
    pit == "244734" ~ "2023", #PIT tag given 2023
    pit == "244792" ~ "2023", #PIT tag given 2023
    pit == "244805" ~ "2023", #PIT tag given 2023
    pit == "244830" ~ "2023", #PIT tag given 2023
    pit == "244863" ~ "2023", #PIT tag given 2023
    pit == "244887" ~ "2019", #noted orange in 2022 with 4 field techs present, red in 2024 with 3 field techs
    pit == "244902" ~ "2017", #noted yellow in 2024, other times VIE was not detected
    pit == "244907" ~ "2022", #PIT tag given 2022 
    .default = first_year
  ))
```

``` r
pits22 <- salamanders %>% 
  filter(pit_year == "2022")

pits23 <- salamanders %>% 
  filter(pit_year == "2023")

pits24 <- salamanders %>% 
  filter(pit_year == "2024",
         pit != "NA")
```

``` r
salamanders <- salamanders %>%
  mutate(pit_year = as.character(pit_year)) %>% 
  mutate(pit_year = case_when(
    pit %in% pits22$pit ~ "2022",
    pit %in% pits23$pit ~ "2023",
    pit %in% pits24$pit ~ "2024",
    .default = pit_year
  ))
```

``` r
write_csv(salamanders, "/Users/georgialattig/Desktop/tidy_otter_point_salamanders.csv")
write.xlsx(salamanders, "/Users/georgialattig/Desktop/tidy_otter_point_salamanders.xlsx")
```
