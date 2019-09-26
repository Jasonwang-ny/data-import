data tidy
================
Jason Wang
9/24/2019

``` r
pulse_data = 
  haven::read_sas("../data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi")
pulse_data_set = mutate(pulse_data, visit = recode(visit, "bl" = "00m"))
```

``` r
litters_data = 
  read_csv("../data/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3)

litters_data_set =  mutate(
    litters_data,
    dose = str_to_lower(dose),
    wt_gain = gd18_weight - gd0_weight)

arrange(litters_data_set, litter_number)
```

    ## # A tibble: 49 x 10
    ##    dose  day_of_tx litter_number gd0_weight gd18_weight gd_of_birth
    ##    <chr> <chr>     <chr>              <dbl>       <dbl>       <int>
    ##  1 con   7         #1/2/95/2           27          42            19
    ##  2 con   7         #1/5/3/83/3-~       NA          NA            20
    ##  3 con   8         #1/6/2/2/95-2       NA          NA            20
    ##  4 mod   7         #1/82/3-2           NA          NA            19
    ##  5 low   8         #100                20          39.2          20
    ##  6 low   7         #101                23.8        42.7          20
    ##  7 low   7         #102                22.6        43.3          20
    ##  8 mod   7         #103                21.4        42.1          19
    ##  9 mod   7         #106                21.7        37.8          20
    ## 10 low   7         #107                22.6        42.4          20
    ## # ... with 39 more rows, and 4 more variables: pups_born_alive <int>,
    ## #   pups_dead_birth <int>, pups_survive <int>, wt_gain <dbl>
