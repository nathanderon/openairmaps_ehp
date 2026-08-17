# Wind rose plots on interactive leaflet maps

`windroseMap()` creates a `leaflet` map using wind roses as markers.
Multiple layers of markers can be added and toggled between using
`control`.

## Usage

``` r
windroseMap(
  data,
  ws.int = 2,
  breaks = 4,
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

  A data frame. The data frame must contain the data to plot a
  [`openair::windRose()`](https://openair-project.github.io/openair/reference/windRose.html),
  which includes wind speed (`ws`), and wind direction (`wd`). In
  addition, `data` must include a decimal latitude and longitude.

- ws.int:

  The wind speed interval. Default is 2 m/s but for low met masts with
  low mean wind speeds a value of 1 or 0.5 m/s may be better.

- breaks:

  Most commonly, the number of break points for wind speed in windRose.
  For windRose and the ws.int default of 2 m/s, the default, 4,
  generates the break points 2, 4, 6, 8 m/s. Breaks can also be used to
  set specific break points. For example, the argument breaks = c(0, 1,
  10, 100) breaks the data into segments \<1, 1-10, 10-100, \>100.

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
  [`openair::windRose`](https://openair-project.github.io/openair/reference/windRose.html)

  `ws`

  :   Name of the column representing wind speed.

  `wd`

  :   Name of the column representing wind direction.

  `ws2,wd2`

  :   The user can supply a second set of wind speed and wind direction
      values with which the first can be compared. See
      [`pollutionRose()`](https://openair-project.github.io/openair/reference/pollutionRose.html)
      for more details.

  `angle`

  :   Default angle of “spokes” is 30. Other potentially useful angles
      are 45 and 10. Note that the width of the wind speed interval may
      need adjusting using `width`.

  `bias.corr`

  :   When `angle` does not divide exactly into 360 a bias is introduced
      in the frequencies when the wind direction is already supplied
      rounded to the nearest 10 degrees, as is often the case. For
      example, if `angle = 22.5`, N, E, S, W will include 3 wind sectors
      and all other angles will be two. A bias correction can made to
      correct for this problem. A simple method according to
      Applequist (2012) is used to adjust the frequencies.

  `grid.line`

  :   Grid line interval to use. If `NULL`, as in default, this is
      assigned based on the available data range. However, it can also
      be forced to a specific value, e.g. `grid.line = 10`. `grid.line`
      can also be a list to control the interval, line type and colour.
      For example
      `grid.line = list(value = 10, lty = 5, col = "purple")`.

  `width`

  :   For `paddle = TRUE`, the adjustment factor for width of wind speed
      intervals. For example, `width = 1.5` will make the paddle width
      1.5 times wider.

  `seg`

  :   When `paddle = TRUE`, `seg` determines with width of the segments.
      For example, `seg = 0.5` will produce segments 0.5 \* `angle`.

  `auto.text`

  :   Either `TRUE` (default) or `FALSE`. If `TRUE` titles and axis
      labels will automatically try and format pollutant names and units
      properly, e.g., by subscripting the ‘2’ in NO2.

  `offset`

  :   The size of the 'hole' in the middle of the plot, expressed as a
      percentage of the polar axis scale, default 10.

  `normalise`

  :   If `TRUE` each wind direction segment is normalised to equal one.
      This is useful for showing how the concentrations (or other
      parameters) contribute to each wind sector when the proportion of
      time the wind is from that direction is low. A line showing the
      probability that the wind directions is from a particular wind
      sector is also shown.

  `max.freq`

  :   Controls the scaling used by setting the maximum value for the
      radial limits. This is useful to ensure several plots use the same
      radial limits.

  `paddle`

  :   Either `TRUE` or `FALSE`. If `TRUE` plots rose using 'paddle'
      style spokes. If `FALSE` plots rose using 'wedge' style spokes.

  `key.header`

  :   Adds additional text/labels above the scale key. For example,
      passing `windRose(mydata, key.header = "ws")` adds the addition
      text as a scale header. Note: This argument is passed to
      `drawOpenKey()` via
      [`quickText()`](https://openair-project.github.io/openair/reference/quickText.html),
      applying the auto.text argument, to handle formatting.

  `key.footer`

  :   Adds additional text/labels below the scale key. See `key.header`
      for further information.

  `key.position`

  :   Location where the scale key is to plotted. Allowed arguments
      currently include “top”, “right”, “bottom” and “left”.

  `dig.lab`

  :   The number of significant figures at which scientific number
      formatting is used in break point and key labelling. Default 5.

  `include.lowest`

  :   Logical. If `FALSE` (the default), the first interval will be left
      exclusive and right inclusive. If `TRUE`, the first interval will
      be left and right inclusive. Passed to the `include.lowest`
      argument of [`cut()`](https://rdrr.io/r/base/cut.html).

  `statistic`

  :   The `statistic` to be applied to each data bin in the plot.
      Options currently include “prop.count”, “prop.mean” and
      “abs.count”. The default “prop.count” sizes bins according to the
      proportion of the frequency of measurements. Similarly,
      “prop.mean” sizes bins according to their relative contribution to
      the mean. “abs.count” provides the absolute count of measurements
      in each bin.

  `pollutant`

  :   Alternative data series to be sampled instead of wind speed. The
      [`windRose()`](https://openair-project.github.io/openair/reference/windRose.html)
      default NULL is equivalent to `pollutant = "ws"`. Use in
      [`pollutionRose()`](https://openair-project.github.io/openair/reference/pollutionRose.html).

  `angle.scale`

  :   The scale is by default shown at a 315 degree angle. Sometimes the
      placement of the scale may interfere with an interesting feature.
      The user can therefore set `angle.scale` to another value (between
      0 and 360 degrees) to mitigate such problems. For example
      `angle.scale = 45` will draw the scale heading in a NE direction.

  `border`

  :   Border colour for shaded areas. Default is no border.

## Value

A leaflet object.

## See also

the original
[`openair::windRose()`](https://openair-project.github.io/openair/reference/windRose.html)

[`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
for the static `ggmap` equivalent of `windroseMap()`

Other interactive directional analysis maps:
[`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md),
[`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md),
[`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md),
[`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md),
[`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md),
[`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
windroseMap(polar_data,
  provider = "Stamen.Toner"
)
} # }
```
