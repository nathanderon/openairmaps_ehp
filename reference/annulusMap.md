# Polar annulus plots on interactive leaflet maps

`annulusMap()` creates a `leaflet` map using polar annulus plots as
markers. Any number of pollutants can be specified using the `pollutant`
argument, and multiple layers of markers can be added and toggled
between using `control`.

## Usage

``` r
annulusMap(
  data,
  pollutant = NULL,
  period = "hour",
  limits = "free",
  latitude = NULL,
  longitude = NULL,
  control = NULL,
  popup = NULL,
  label = NULL,
  provider = "OpenStreetMap",
  cols = "turbo",
  alpha = 1,
  key = FALSE,
  draw.legend = TRUE,
  collapse.control = FALSE,
  d.icon = 200,
  d.fig = 3.5,
  type = deprecated(),
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
  are specified, they can be toggled between using a "layer control"
  interface.

- period:

  This determines the temporal period to consider. Options are "hour"
  (the default, to plot diurnal variations), "season" to plot variation
  throughout the year, "weekday" to plot day of the week variation and
  "trend" to plot the trend by wind direction.

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

- control:

  Used for splitting the input data into different groups which can be
  selected between using a "layer control" interface, passed to the
  `type` argument of
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).
  `control` cannot be used if multiple `pollutant` columns have been
  provided.

- popup:

  Columns to be used as the HTML content for marker popups. Popups may
  be useful to show information about the individual sites (e.g., site
  names, codes, types, etc.). If a vector of column names are provided
  they are passed to
  [`buildPopup()`](https://davidcarslaw.github.io/openairmaps/reference/buildPopup.md)
  using its default values.

- label:

  Column to be used as the HTML content for hover-over labels. Labels
  are useful for the same reasons as popups, though are typically
  shorter.

- provider:

  The base map(s) to be used. See
  <http://leaflet-extras.github.io/leaflet-providers/preview/> for a
  list of all base maps that can be used. If multiple base maps are
  provided, they can be toggled between using a "layer control"
  interface. By default, the interface will use the provider names as
  labels, but users can define their own using a named vector (e.g.,
  `c("Default" = "OpenStreetMap", "Satellite" = "Esri.WorldImagery")`)

- cols:

  The colours used for plotting. See
  [`openair::openColours()`](https://openair-project.github.io/openair/reference/openColours.html)
  for more information.

- alpha:

  The alpha transparency to use for the plotting surface (a value
  between 0 and 1 with zero being fully transparent and 1 fully opaque).

- key:

  Should a key for each marker be drawn? Default is `FALSE`.

- draw.legend:

  When `limits` are specified, should a shared legend be created at the
  side of the map? Default is `TRUE`.

- collapse.control:

  Should the "layer control" interface be collapsed? Defaults to
  `FALSE`.

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

- type:

  **\[deprecated\]**. Different sites are now automatically detected
  based on latitude and longitude. Please use `label` and/or `popup` to
  label different sites.

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

A leaflet object.

## See also

the original
[`openair::polarAnnulus()`](https://openair-project.github.io/openair/reference/polarAnnulus.html)

[`annulusMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMapStatic.md)
for the static `ggmap` equivalent of `annulusMap()`

Other interactive directional analysis maps:
[`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md),
[`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md),
[`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md),
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md),
[`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md),
[`windroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
annulusMap(polar_data,
  pollutant = "nox",
  period = "hour",
  provider = "Stamen.Toner"
)
} # }
```
