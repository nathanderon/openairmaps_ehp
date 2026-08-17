# Percentile roses on interactive leaflet maps

`percentileMap()` creates a `leaflet` map using percentile roses as
markers. Any number of pollutants can be specified using the `pollutant`
argument, and multiple layers of markers can be added and toggled
between using `control`.

## Usage

``` r
percentileMap(
  data,
  pollutant = NULL,
  percentile = c(25, 50, 75, 90, 95),
  intervals = "fixed",
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

- percentile:

  The percentile value(s) to plot. Must be between 0–100. If
  `percentile = NA` then only a mean line will be shown.

- intervals:

  One of:

  - `"fixed"` (the default) which ensures all of the markers use the
    same radial axis scale.

  - `"free"` which allows all of the markers to use different radial
    axis scales.

  - A numeric vector defining a sequence of numbers to use as the
    intervals, e.g., `intervals = c(0, 10, 30, 50)`.

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

  Should a shared legend be created at the side of the map? Default is
  `TRUE`.

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
  [`openair::percentileRose`](https://openair-project.github.io/openair/reference/percentileRose.html)

  `wd`

  :   Name of wind direction field.

  `smooth`

  :   Should the wind direction data be smoothed using a cyclic spline?

  `method`

  :   When `method = "default"` the supplied percentiles by wind
      direction are calculated. When `method = "cpf"` the conditional
      probability function (CPF) is plotted and a single (usually high)
      percentile level is supplied. The CPF is defined as CPF = my/ny,
      where my is the number of samples in the wind sector y with mixing
      ratios greater than the *overall* percentile concentration, and ny
      is the total number of samples in the same wind sector (see
      Ashbaugh et al., 1985).

  `angle`

  :   Default angle of “spokes” is when `smooth = FALSE`.

  `mean`

  :   Show the mean by wind direction as a line?

  `mean.lty`

  :   Line type for mean line.

  `mean.lwd`

  :   Line width for mean line.

  `mean.col`

  :   Line colour for mean line.

  `fill`

  :   Should the percentile intervals be filled (default) or should
      lines be drawn (`fill = FALSE`).

  `angle.scale`

  :   Sometimes the placement of the scale may interfere with an
      interesting feature. The user can therefore set `angle.scale` to
      any value between 0 and 360 degrees to mitigate such problems. For
      example `angle.scale = 45` will draw the scale heading in a NE
      direction.

  `auto.text`

  :   Either `TRUE` (default) or `FALSE`. If `TRUE` titles and axis
      labels will automatically try and format pollutant names and units
      properly e.g. by subscripting the \`2' in NO2.

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

## Value

A leaflet object.

## See also

the original
[`openair::percentileRose()`](https://openair-project.github.io/openair/reference/percentileRose.html)

[`percentileMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMapStatic.md)
for the static `ggmap` equivalent of `percentileMap()`

Other interactive directional analysis maps:
[`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md),
[`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md),
[`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md),
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md),
[`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md),
[`windroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
percentileMap(polar_data,
  pollutant = "nox",
  provider = "Stamen.Toner"
)
} # }
```
