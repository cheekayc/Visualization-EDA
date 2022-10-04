Visualization_2
================
Lectured by Jeff Goldsmith
2022-10-04

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2022-09-29 10:29:25 (8.401)

    ## file min/max dates: 1869-01-01 / 2022-09-30

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2022-09-29 10:29:32 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: ~/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2022-09-29 10:29:35 (0.95)

    ## file min/max dates: 1999-09-01 / 2022-09-30

# Scatterplot

But better this time

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    x = "Minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily temp extremes",
    caption = "Data come from the rnoaa package") +
  scale_x_continuous(
    breaks = c(-10, 0, 15),    # can adjust how your scale looks like.
    labels = c("-10C", "0C", "15C")) +  # label your scale.
  scale_y_continuous(
    trans = "sqrt", 
    position = "right")
```

    ## Warning in self$trans$transform(x): NaNs produced

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 90 rows containing missing values (geom_point).

![](Visualization_2_files/figure-gfm/plot%201-1.png)<!-- --> `labs()`
function is labelling the plots.

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    x = "Minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily temp extremes",
    caption = "Data come from the rnoaa package") +
  scale_color_hue(
    name = "Location",   # changed the variable "name" to "Location".
    h = c(100, 300))  # Not encourage to do this. Color is very ugly.
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](Visualization_2_files/figure-gfm/plot%202-1.png)<!-- -->

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    x = "Minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily temp extremes",
    caption = "Data come from the rnoaa package") +
  viridis::scale_color_viridis(     # This color pallate is good.
    name = "Location",   # changed the variable "name" to "Location".
    discrete = TRUE)
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](Visualization_2_files/figure-gfm/viridis%20color-1.png)<!-- -->

# Themes

``` r
ggp_weather = 
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) + 
  geom_point(alpha = .5) +
  labs(
    x = "Minimum Daily Temp (C)",
    y = "Maximum Daily Temp (C)",
    title = "Scatterplot of daily temp extremes",
    caption = "Data come from the rnoaa package") +
  viridis::scale_color_viridis(     # This color pallate is good.
    name = "Location",   # changed the variable "name" to "Location".
    discrete = TRUE)
```

``` r
ggp_weather + 
  theme_bw() + # change how the background grid looks like. Try _classic, _minimal, etc.
  theme(legend.position = "bottom")  # change legend position to bottom.
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](Visualization_2_files/figure-gfm/legend%20position-1.png)<!-- -->

``` r
# Must change grid theme first, then legend position later. Otherwise, theme position would not change.
```

# Setting how my plots will look.

Set these options at the beginning of your document, so you don’t have
to write the same code over and over again in every single code chunk.

``` r
library(tidyverse)

knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)

theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

# 
