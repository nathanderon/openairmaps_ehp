# openairmaps: tools to create maps of air pollution data

The main goal of
[openairmaps](https://openair-project.github.io/openairmaps/) is to
combine the robust analytical methods found in
[openair](https://davidcarslaw.github.io/openair/) with the highly
capable [leaflet](https://rstudio.github.io/leaflet/) package.
[openairmaps](https://openair-project.github.io/openairmaps/) is
thoroughly documented in the [openair
book](https://bookdown.org/david_carslaw/openair/sections/maps/maps-overview.html).

## Installation

You can install the release version of
[openairmaps](https://openair-project.github.io/openairmaps/) from CRAN
with:

``` r

install.packages("openairmaps")
```

You can install the development version of
[openairmaps](https://openair-project.github.io/openairmaps/) from
GitHub with:

``` r

# install.packages("pak")
pak::pak("davidcarslaw/openairmaps")
```

## Overview

``` r

library(openairmaps)
```

The `openairmaps` package is thoroughly documented in the [openair
book](https://bookdown.org/david_carslaw/openair/sections/maps/maps-overview.html),
which goes into great detail about its various functions. Functionality
includes visualising UK AQ networks
([`networkMap()`](https://davidcarslaw.github.io/openairmaps/reference/networkMap.md)),
putting “polar directional markers” on maps (e.g.,
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md))
and overlaying HYSPLIT trajectories on maps (e.g.,
[`trajMap()`](https://davidcarslaw.github.io/openairmaps/reference/trajMap.md)),
all using the [leaflet](https://rstudio.github.io/leaflet/) package.

``` r

polar_data %>%
  openair::cutData("daylight") %>%
  buildPopup(
    c("site", "site_type"),
    names = c("Site" = "site", "Site Type" = "site_type"),
    control = "daylight"
  ) %>%
  polarMap(
    pollutant = "no2",
    limits = c(0, 180),
    control = "daylight",
    popup = "popup"
  )
```

![A screenshot of a leaflet map. It shows an OpenStreetMap map layer,
overlaid with bivariate polar plots. Polar plots are visualisations on
polar coordinates with wind direction on the spoke axes, wind speed on
the radial axes, and a smooth surface showing pollutant concentrations.
A menu is found at the top-right of the map, which allows users to swap
between daylight and nighttime
observations.](reference/figures/README-examplemap.png)

An example
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md)
showing NO2 concentrations in central London.

While an interactive map is preferred for exploratory directional
analysis, it is limited to the HTML format. Some applications (for
example, academic journals) demand “static” formats like .docx and .pdf.
For this reason, “static” versions of
[openairmaps](https://openair-project.github.io/openairmaps/) polar
marker functions have been provided which are written in
[ggplot2](https://ggplot2.tidyverse.org). A benefit of being written in
[ggplot2](https://ggplot2.tidyverse.org) is that additional layers can
be added (e.g., `geom_label()` could be used to label sites) and limited
further customisation is available using `theme()` and `guides()`.

Static maps require users to provide a
[ggmap](https://github.com/dkahle/ggmap) tileset, which at the time of
writing requires an API key for either Google or Stadia Maps.
