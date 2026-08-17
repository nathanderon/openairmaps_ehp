# Bivariate polar plots on a static ggmap

`annulusMapStatic()` creates a `ggplot2` map using polar annulus plots
as markers. As this function returns a `ggplot2` object, further
customisation can be achieved using functions like
[`ggplot2::theme()`](https://ggplot2.tidyverse.org/reference/theme.html)
and
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html).

## Usage

``` r
annulusMapStatic(
  data,
  pollutant = NULL,
  ggmap,
  period = "hour",
  facet = NULL,
  limits = "free",
  latitude = NULL,
  longitude = NULL,
  cols = "turbo",
  alpha = 1,
  key = FALSE,
  facet.nrow = NULL,
  d.icon = 150,
  d.fig = 3,
  ...
)
```

## Arguments

- data:

  A data frame. The data frame must contain the data to plot the
  directional analysis marker, which includes wind speed (`ws`), wind
  direction (`wd`), and the column representing the concentration of a
  pollutant. In addition, `data` must include a decimal latitude and
  longitude.

- pollutant:

  The column name(s) of the pollutant(s) to plot. If multiple pollutants
  are specified, they will each form part of a separate panel.

- ggmap:

  A `ggmap` object obtained using
  [`ggmap::get_map()`](https://rdrr.io/pkg/ggmap/man/get_map.html) or a
  similar function to use as the basemap.

- period:

  This determines the temporal period to consider. Options are "hour"
  (the default, to plot diurnal variations), "season" to plot variation
  throughout the year, "weekday" to plot day of the week variation and
  "trend" to plot the trend by wind direction.

- facet:

  Used for splitting the input data into different panels, passed to the
  `type` argument of
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).
  `facet` cannot be used if multiple `pollutant` columns have been
  provided.

- limits:

  One of:

  - `"fixed"` which ensures all of the markers use the same colour
    scale.

  - `"free"` (the default) which allows all of the markers to use
    different colour scales.

  - A numeric vector in the form `c(lower, upper)` used to define the
    colour scale. For example, `limits = c(0, 100)` would force the plot
    limits to span 0-100.

- latitude, longitude:

  The decimal latitude/longitude. If not provided, will be automatically
  inferred from data by looking for a column named "lat"/"latitude" or
  "lon"/"lng"/"long"/"longitude" (case-insensitively).

- cols:

  The colours used for plotting. See
  [`openair::openColours()`](https://openair-project.github.io/openair/reference/openColours.html)
  for more information.

- alpha:

  The alpha transparency to use for the plotting surface (a value
  between 0 and 1 with zero being fully transparent and 1 fully opaque).

- key:

  Should a key for each marker be drawn? Default is `FALSE`.

- facet.nrow:

  Passed to the `nrow` argument of
  [`ggplot2::facet_wrap()`](https://ggplot2.tidyverse.org/reference/facet_wrap.html).

- d.icon:

  The diameter of the plot on the map in pixels. This will affect the
  size of the individual polar markers. Alternatively, a vector in the
  form `c(width, height)` can be provided if a non-circular marker is
  desired.

- d.fig:

  The diameter of the plots to be produced using `openair` in inches.
  This will affect the resolution of the markers on the map.
  Alternatively, a vector in the form `c(width, height)` can be provided
  if a non-circular marker is desired.

- ...:

  Arguments passed on to
  [`openair::polarAnnulus`](https://openair-project.github.io/openair/reference/polarAnnulus.html)

  `resolution`

  :   Two plot resolutions can be set: “normal” and “fine” (the
      default).

  `local.tz`

  :   Should the results be calculated in local time that includes a
      treatment of daylight savings time (DST)? The default is not to
      consider DST issues, provided the data were imported without a DST
      offset. Emissions activity tends to occur at local time e.g. rush
      hour is at 8 am every day. When the clocks go forward in spring,
      the emissions are effectively released into the atmosphere
      typically 1 hour earlier during the summertime i.e. when DST
      applies. When plotting diurnal profiles, this has the effect of
      “smearing-out” the concentrations. Sometimes, a useful approach is
      to express time as local time. This correction tends to produce
      better-defined diurnal profiles of concentration (or other
      variables) and allows a better comparison to be made with
      emissions/activity data. If set to `FALSE` then GMT is used.
      Examples of usage include `local.tz = "Europe/London"`,
      `local.tz = "America/New_York"`. See `cutData` and `import` for
      more details.

  `type`

  :   `type` determines how the data are split i.e. conditioned, and
      then plotted. The default is will produce a single plot using the
      entire data. Type can be one of the built-in types as detailed in
      `cutData` e.g. “season”, “year”, “weekday” and so on. For example,
      `type = "season"` will produce four plots — one for each season.

      It is also possible to choose `type` as another variable in the
      data frame. If that variable is numeric, then the data will be
      split into four quantiles (if possible) and labelled accordingly.
      If type is an existing character or factor variable, then those
      categories/levels will be used directly. This offers great
      flexibility for understanding the variation of different variables
      and how they depend on one another.

      Type can be up length two e.g. `type = c("season", "site")` will
      produce a 2x2 plot split by season and site. The use of two types
      is mostly meant for situations where there are several sites.
      Note, when two types are provided the first forms the columns and
      the second the rows.

      Also note that for the `polarAnnulus` function some type/period
      combinations are forbidden or make little sense. For example,
      `type = "season"` and `period = "trend"` (which would result in a
      plot with too many gaps in it for sensible smoothing), or
      `type = "weekday"` and `period = "weekday"`.

  `statistic`

  :   The statistic that should be applied to each wind speed/direction
      bin. Can be “mean” (default), “median”, “max” (maximum),
      “frequency”. “stdev” (standard deviation), “weighted.mean” or
      “cpf” (Conditional Probability Function). Because of the smoothing
      involved, the colour scale for some of these statistics is only to
      provide an indication of overall pattern and should not be
      interpreted in concentration units e.g. for
      `statistic = "weighted.mean"` where the bin mean is multiplied by
      the bin frequency and divided by the total frequency. In many
      cases using `polarFreq` will be better. Setting
      `statistic = "weighted.mean"` can be useful because it provides an
      indication of the concentration \* frequency of occurrence and
      will highlight the wind speed/direction conditions that dominate
      the overall mean.

  `percentile`

  :   If `statistic = "percentile"` or `statistic = "cpf"` then
      `percentile` is used, expressed from 0 to 100. Note that the
      percentile value is calculated in the wind speed, wind direction
      ‘bins’. For this reason it can also be useful to set `min.bin` to
      ensure there are a sufficient number of points available to
      estimate a percentile. See `quantile` for more details of how
      percentiles are calculated.

  `width`

  :   The width of the annulus; can be “normal” (the default), “thin” or
      “fat”.

  `min.bin`

  :   The minimum number of points allowed in a wind speed/wind
      direction bin. The default is 1. A value of two requires at least
      2 valid records in each bin an so on; bins with less than 2 valid
      records are set to NA. Care should be taken when using a value \>
      1 because of the risk of removing real data points. It is
      recommended to consider your data with care. Also, the `polarFreq`
      function can be of use in such circumstances.

  `exclude.missing`

  :   Setting this option to `TRUE` (the default) removes points from
      the plot that are too far from the original data. The smoothing
      routines will produce predictions at points where no data exist
      i.e. they predict. By removing the points too far from the
      original data produces a plot where it is clear where the original
      data lie. If set to `FALSE` missing data will be interpolated.

  `date.pad`

  :   For `type = "trend"` (default), `date.pad = TRUE` will pad-out
      missing data to the beginning of the first year and the end of the
      last year. The purpose is to ensure that the trend plot begins and
      ends at the beginning or end of year.

  `force.positive`

  :   The default is `TRUE`. Sometimes if smoothing data with steep
      gradients it is possible for predicted values to be negative.
      `force.positive = TRUE` ensures that predictions remain positive.
      This is useful for several reasons. First, with lots of missing
      data more interpolation is needed and this can result in artefacts
      because the predictions are too far from the original data.
      Second, if it is known beforehand that the data are all positive,
      then this option carries that assumption through to the
      prediction. The only likely time where setting
      `force.positive = FALSE` would be if background concentrations
      were first subtracted resulting in data that is legitimately
      negative. For the vast majority of situations it is expected that
      the user will not need to alter the default option.

  `k`

  :   The smoothing value supplied to `gam` for the temporal and wind
      direction components, respectively. In some cases e.g. a trend
      plot with less than 1-year of data the smoothing with the default
      values may become too noisy and affected more by outliers.
      Choosing a lower value of `k` (say 10) may help produce a better
      plot.

  `normalise`

  :   If `TRUE` concentrations are normalised by dividing by their mean
      value. This is done *after* fitting the smooth surface. This
      option is particularly useful if one is interested in the patterns
      of concentrations for several pollutants on different scales e.g.
      NOx and CO. Often useful if more than one `pollutant` is chosen.

  `key.header`

  :   Adds additional text/labels to the scale key. For example, passing
      the options `key.header = "header", key.footer = "footer1"` adds
      addition text above and below the scale key. These arguments are
      passed to `drawOpenKey` via `quickText`, applying the `auto.text`
      argument, to handle formatting.

  `key.footer`

  :   see `key.footer`.

  `key.position`

  :   Location where the scale key is to plotted. Allowed arguments
      currently include `"top"`, `"right"`, `"bottom"` and `"left"`.

  `auto.text`

  :   Either `TRUE` (default) or `FALSE`. If `TRUE` titles and axis
      labels will automatically try and format pollutant names and units
      properly e.g. by subscripting the \`2' in NO2.

## Value

a `ggplot2` plot with a `ggmap` basemap

## Further customisation using ggplot2

As the outputs of the static directional analysis functions are
`ggplot2` figures, further customisation is possible using functions
such as
[`ggplot2::theme()`](https://ggplot2.tidyverse.org/reference/theme.html),
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html)
and
[`ggplot2::labs()`](https://ggplot2.tidyverse.org/reference/labs.html).

If multiple pollutants are specified, subscripting (e.g., the "x" in
"NOx") is achieved using the
[ggtext](https://wilkelab.org/ggtext/reference/ggtext.html) package.
Therefore if you choose to override the plot theme, it is recommended to
use `[ggplot2::theme()]` and `[ggtext::element_markdown()]` to define
the `strip.text` parameter.

When arguments like `limits`, `percentile` or `breaks` are defined, a
legend is automatically added to the figure. Legends can be removed
using `ggplot2::theme(legend.position = "none")`, or further customised
using
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html)
and either `color = ggplot2::guide_colourbar()` for continuous legends
or `fill = ggplot2::guide_legend()` for discrete legends.

## See also

the original
[`openair::polarAnnulus()`](https://openair-project.github.io/openair/reference/polarAnnulus.html)

[`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md)
for the interactive `leaflet` equivalent of `annulusMapStatic()`

Other static directional analysis maps:
[`diffMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/diffMapStatic.md),
[`freqMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/freqMapStatic.md),
[`percentileMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMapStatic.md),
[`polarMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/polarMapStatic.md),
[`pollroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMapStatic.md),
[`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
