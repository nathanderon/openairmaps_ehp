# Polar frequency plots on interactive leaflet maps

`freqMap()` creates a `leaflet` map using binned polar plots as markers.
Any number of pollutants can be specified using the `pollutant`
argument, and multiple layers of markers can be added and toggled
between using `control`.

## Usage

``` r
freqMap(
  data,
  pollutant = NULL,
  statistic = "mean",
  breaks = "free",
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

- statistic:

  The statistic that should be applied to each wind speed/direction bin.
  Can be "frequency", "mean", "median", "max" (maximum), "stdev"
  (standard deviation) or "weighted.mean". The option "frequency" is the
  simplest and plots the frequency of wind speed/direction in different
  bins. The scale therefore shows the counts in each bin. The option
  "mean" (the default) will plot the mean concentration of a pollutant
  (see next point) in wind speed/direction bins, and so on. Finally,
  "weighted.mean" will plot the concentration of a pollutant weighted by
  wind speed/direction. Each segment therefore provides the percentage
  overall contribution to the total concentration. Note that for options
  other than "frequency", it is necessary to also provide the name of a
  pollutant. See function
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html)
  for further details.

- breaks:

  One of:

  - `"fixed"` which ensures all of the markers use the same colour
    scale.

  - `"free"` (the default) which allows all of the markers to use
    different colour scales.

  - A numeric vector defining a sequence of numbers to use as the
    breaks. The sequence could represent one with equal spacing, e.g.,
    `breaks = seq(0, 100, 10)` - a scale from 0-10 in intervals of 10,
    or a more flexible sequence, e.g., `breaks = c(0, 1, 5, 7, 10)`,
    which may be useful for some situations.

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

  When `breaks` are specified, should a shared legend be created at the
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
  [`openair::polarFreq`](https://openair-project.github.io/openair/reference/polarFreq.html)

  `ws.int`

  :   Wind speed interval assumed. In some cases e.g. a low met mast, an
      interval of 0.5 may be more appropriate.

  `wd.nint`

  :   Number of intervals of wind direction.

  `grid.line`

  :   Radial spacing of grid lines.

  `trans`

  :   Should a transformation be applied? Sometimes when producing plots
      of this kind they can be dominated by a few high points. The
      default therefore is `TRUE` and a square-root transform is
      applied. This results in a non-linear scale and (usually) a better
      representation of the distribution. If set to `FALSE` a linear
      scale is used.

  `min.bin`

  :   The minimum number of points allowed in a wind speed/wind
      direction bin. The default is 1. A value of two requires at least
      2 valid records in each bin an so on; bins with less than 2 valid
      records are set to NA. Care should be taken when using a value \>
      1 because of the risk of removing real data points. It is
      recommended to consider your data with care. Also, the `polarFreq`
      function can be of use in such circumstances.

  `ws.upper`

  :   A user-defined upper wind speed to use. This is useful for
      ensuring a consistent scale between different plots. For example,
      to always ensure that wind speeds are displayed between 1-10, set
      `ws.int = 10`.

  `offset`

  :   `offset` controls the size of the ‘hole’ in the middle and is
      expressed as a percentage of the maximum wind speed. Setting a
      higher `offset` e.g. 50 is useful for
      `statistic = "weighted.mean"` when `ws.int` is greater than the
      maximum wind speed. See example below.

  `border.col`

  :   The colour of the boundary of each wind speed/direction bin. The
      default is transparent. Another useful choice sometimes is
      "white".

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
[`openair::polarFreq()`](https://openair-project.github.io/openair/reference/polarFreq.html)

[`freqMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/freqMapStatic.md)
for the static `ggmap` equivalent of `freqMap()`

Other interactive directional analysis maps:
[`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md),
[`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md),
[`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md),
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md),
[`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md),
[`windroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
freqMap(polar_data,
  pollutant = "nox",
  statistic = "mean",
  provider = "Stamen.Toner"
)
} # }
```
