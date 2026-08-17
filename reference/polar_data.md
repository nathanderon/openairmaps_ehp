# Example data for polar mapping functions

The `polar_data` dataset is provided as an example dataset as part of
the `openairmaps` package. The dataset contains hourly measurements of
air pollutant concentrations, location and meteorological data.

## Format

Data frame with example data from four sites in London in 2009.

- date:

  The date and time of the measurement

- nox, no2, pm2.5, pm10:

  Pollutant concentrations

- site:

  The site name. Useful for use with the `popup` and `label` arguments
  in `openairmaps` functions.

- latitude, longitude:

  Decimal latitude and longitude of the sites.

- site.type:

  Site type of the site (either "Urban Traffic" or "Urban Background").

- wd:

  Wind direction, in degrees from North, as a numeric vector.

- ws:

  Wind speed, in m/s, as numeric vector.

- visibility:

  The visibility in metres.

- air_temp:

  Air temperature in degrees Celcius.

## Source

`polar_data` was compiled from data using the
[`openair::importAURN()`](https://openair-project.github.io/openair/reference/importUKAQ-wrapper.html)
function from the `openair` package with meteorological data from the
`worldmet` package.

## Details

`polar_data` is supplied with the `openairmaps` package as an example
dataset for use with documented examples.

## Examples

``` r

# basic structure
head(polar_data)
#> # A tibble: 6 × 13
#>   date                  nox   no2 pm2.5  pm10 site    lat    lon site_type    wd
#>   <dttm>              <dbl> <dbl> <dbl> <dbl> <chr> <dbl>  <dbl> <chr>     <dbl>
#> 1 2009-01-01 00:00:00   113    46    42    46 Lond…  51.5 -0.126 Urban Ba…  58.9
#> 2 2009-01-01 01:00:00    40    32    45    49 Lond…  51.5 -0.126 Urban Ba…  74.5
#> 3 2009-01-01 02:00:00    48    36    43    46 Lond…  51.5 -0.126 Urban Ba…  30  
#> 4 2009-01-01 03:00:00    36    29    37    NA Lond…  51.5 -0.126 Urban Ba…  45  
#> 5 2009-01-01 04:00:00    40    32    36    38 Lond…  51.5 -0.126 Urban Ba…  70  
#> 6 2009-01-01 05:00:00    50    36    33    32 Lond…  51.5 -0.126 Urban Ba…  46.6
#> # ℹ 3 more variables: ws <dbl>, visibility <dbl>, air_temp <dbl>
```
