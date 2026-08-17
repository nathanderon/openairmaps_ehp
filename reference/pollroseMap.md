# Pollution rose plots on interactive leaflet maps

`pollroseMap()` creates a `leaflet` map using "pollution roses" as
markers. Any number of pollutants can be specified using the `pollutant`
argument, and multiple layers of markers can be added and toggled
between using `control`.

## Usage

``` r
pollroseMap(
  data,
  pollutant = NULL,
  statistic = "prop.count",
  breaks = NULL,
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

  The `statistic` to be applied to each data bin in the plot. Options
  currently include "prop.count", "prop.mean" and "abs.count". The
  default "prop.count" sizes bins according to the proportion of the
  frequency of measurements. Similarly, "prop.mean" sizes bins according
  to their relative contribution to the mean. "abs.count" provides the
  absolute count of measurements in each bin.

- breaks:

  Most commonly, the number of break points. If not specified, each
  marker will independently break its supplied data at approximately 6
  sensible break points. When `breaks` are specified, all markers will
  use the same break points. Breaks can also be used to set specific
  break points. For example, the argument `breaks = c(0, 1, 10, 100)`
  breaks the data into segments \<1, 1-10, 10-100, \>100.

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
  [`openair::pollutionRose`](https://openair-project.github.io/openair/reference/pollutionRose.html)

  `key.footer`

  :   Adds additional text/labels below the scale key. See `key.header`
      for further information.

  `key.position`

  :   Location where the scale key is to plotted. Allowed arguments
      currently include “top”, “right”, “bottom” and “left”.

  `paddle`

  :   Either `TRUE` or `FALSE`. If `TRUE` plots rose using 'paddle'
      style spokes. If `FALSE` plots rose using 'wedge' style spokes.

  `seg`

  :   When `paddle = TRUE`, `seg` determines with width of the segments.
      For example, `seg = 0.5` will produce segments 0.5 \* `angle`.

  `normalise`

  :   If `TRUE` each wind direction segment is normalised to equal one.
      This is useful for showing how the concentrations (or other
      parameters) contribute to each wind sector when the proportion of
      time the wind is from that direction is low. A line showing the
      probability that the wind directions is from a particular wind
      sector is also shown.

## Value

A leaflet object.

## See also

the original
[`openair::pollutionRose()`](https://openair-project.github.io/openair/reference/pollutionRose.html)

[`pollroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMapStatic.md)
for the static `ggmap` equivalent of `pollroseMap()`

Other interactive directional analysis maps:
[`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md),
[`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md),
[`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md),
[`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md),
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md),
[`windroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
pollroseMap(polar_data,
  pollutant = "nox",
  statistic = "prop.count",
  provider = "Stamen.Toner"
)
} # }
```
