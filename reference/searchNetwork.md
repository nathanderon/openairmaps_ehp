# Geographically search the air quality networks made available by [`openair::importMeta()`](https://openair-project.github.io/openair/reference/importMeta.html)

While
[`networkMap()`](https://davidcarslaw.github.io/openairmaps/reference/networkMap.md)
visualises entire UK air quality networks, `searchNetwork()` can subset
specific networks to find air quality sites near to a specific site of
interest (for example, the location of known industrial activity, or the
centroid of a specific urban area).

## Usage

``` r
searchNetwork(
  lat,
  lng,
  source = "aurn",
  year = NULL,
  site_type = NULL,
  variable = NULL,
  max_dist = NULL,
  n = NULL,
  map = TRUE
)
```

## Arguments

- lat, lng:

  The latitude and longitude for the location of interest.

- source:

  One or more air quality networks for which data is available through
  openair. Available networks include:

  - `"aurn"`, The UK Automatic Urban and Rural Network.

  - `"aqe"`, The Air Quality England Network.

  - `"saqn"`, The Scottish Air Quality Network.

  - `"waqn"`, The Welsh Air Quality Network.

  - `"ni"`, The Northern Ireland Air Quality Network.

  - `"local"`, Locally managed air quality networks in England.

  - `"kcl"`, King's College London networks.

  - `"europe"`, European AirBase/e-reporting data. There are two
    additional options provided for convenience:

  - `"ukaq"` will return metadata for all networks for which data is
    imported by
    [`importUKAQ()`](https://openair-project.github.io/openair/reference/importUKAQ.html)
    (i.e., AURN, AQE, SAQN, WAQN, NI, and the local networks).

  - `"all"` will import all available metadata (i.e., `"ukaq"` plus
    `"kcl"` and `"europe"`).

- year:

  If a single year is selected, only sites that were open at some point
  in that year are returned. If `all = TRUE` only sites that measured a
  particular pollutant in that year are returned. Year can also be a
  sequence e.g. `year = 2010:2020` or of length 2 e.g.
  `year = c(2010, 2020)`, which will return only sites that were open
  over the duration. Note that `year` is ignored when the `source` is
  either `"kcl"` or `"europe"`.

- site_type:

  Optional. One or more site types with which to subset the site
  metadata. For example, `site_type = "urban background"` will only
  search urban background sites.

- variable:

  Optional. One or more variables of interest with which to subset the
  site metadata. For example, `variable = c("pm10", "co")` will search
  sites that measure PM10 and/or CO.

- max_dist:

  Optional. A maximum distance from the location of interest in
  kilometres.

- n:

  Optional. The maximum number of sites to return.

- map:

  If `TRUE`, the default, `searchNetwork()` will return a `leaflet` map.
  If `FALSE`, it will instead return a
  [tibble](https://tibble.tidyverse.org/reference/tibble-package.html).

## Value

Either a
[tibble](https://tibble.tidyverse.org/reference/tibble-package.html) or
`leaflet` map.

## Details

Data subsetting progresses in the order in which the arguments are
given; first `source` and `year`, then `site_type` and `variable`, then
`max_dist`, and finally `n`.

## See also

Other uk air quality network mapping functions:
[`networkMap()`](https://davidcarslaw.github.io/openairmaps/reference/networkMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# get all AURN sites open in 2020 within 20 km of Buckingham Palace
palace <- convertPostcode("SW1A1AA")
searchNetwork(lat = palace$lat, lng = palace$lng, max_dist = 20, year = 2020)
} # }
```
