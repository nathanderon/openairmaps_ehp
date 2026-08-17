# Trajectory line plots in `ggplot2`

**\[experimental\]**

This function plots back trajectories using `ggplot2`. The function
requires that data are imported using
[`openair::importTraj()`](https://openair-project.github.io/openair/reference/importTraj.html).
It is a `ggplot2` implementation of
[`openair::trajPlot()`](https://openair-project.github.io/openair/reference/trajPlot.html)
with many of the same arguments, which should be more flexible for
post-hoc changes.

## Usage

``` r
trajMapStatic(
  data,
  colour = "height",
  facet = NULL,
  group = NULL,
  longitude = "lon",
  latitude = "lat",
  npoints = 12,
  xlim = NULL,
  ylim = NULL,
  crs = sf::st_crs(3812),
  origin = TRUE,
  map = TRUE,
  map.fill = "grey85",
  map.colour = "grey75",
  map.alpha = 0.8,
  map.lwd = 0.5,
  map.lty = 1,
  ...
)
```

## Arguments

- data:

  Data frame, the result of importing a trajectory file using
  [`openair::importTraj()`](https://openair-project.github.io/openair/reference/importTraj.html).

- colour:

  Column to be used for colouring each trajectory. This column may be
  numeric, character or factor. This will commonly be a pollutant
  concentration which has been joined (e.g., by
  [`dplyr::left_join()`](https://dplyr.tidyverse.org/reference/mutate-joins.html))
  to the trajectory data by "date".

- facet:

  Used for splitting the trajectories into different panels. Passed to
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).

- group:

  By default, trajectory paths are distinguished using the arrival date.
  `group` allows for additional columns to be used (e.g., `"receptor"`).

- latitude, longitude:

  The decimal latitude/longitude.

- npoints:

  A dot is placed every `npoints` along each full trajectory. For hourly
  back trajectories points are plotted every `npoints` hours. This helps
  to understand where the air masses were at particular times and get a
  feel for the speed of the air (points closer together correspond to
  slower moving air masses). Defaults to `12`.

- xlim, ylim:

  The x- and y-limits of the plot. If `NULL`, limits will be estimated
  based on the lat/lon ranges of the input data.

- crs:

  The coordinate reference system (CRS) into which all data should be
  projected before plotting. Defaults to the Lambert projection
  (`sf::st_crs(3812)`). Alternatively, can be set to `NULL`, which will
  typically render the map quicker but may cause countries far from the
  equator or large areas to appear distorted.

- origin:

  Should the receptor point be marked with a circle? Defaults to `TRUE`.

- map:

  Should a base map be drawn? Defaults to `TRUE`.

- map.fill:

  Colour to use to fill the polygons of the base map (see
  [`colors()`](https://rdrr.io/r/grDevices/colors.html)).

- map.colour:

  Colour to use for the polygon borders of the base map (see
  [`colors()`](https://rdrr.io/r/grDevices/colors.html)).

- map.alpha:

  Transparency of the base map polygons. Must be between `0` (fully
  transparent) and `1` (fully opaque).

- map.lwd:

  Line width of the base map polygon borders.

- map.lty:

  Line type of the base map polygon borders. See
  [`ggplot2::scale_linetype()`](https://ggplot2.tidyverse.org/reference/scale_linetype.html)
  for common examples.

- ...:

  Arguments passed on to
  [`ggplot2::coord_sf`](https://ggplot2.tidyverse.org/reference/ggsf.html)

  `expand`

  :   If `TRUE`, the default, adds a small expansion factor to the
      limits to ensure that data and axes don't overlap. If `FALSE`,
      limits are taken exactly from the data or `xlim`/`ylim`.

  `datum`

  :   CRS that provides datum to use when generating graticules.

  `label_graticule`

  :   Character vector indicating which graticule lines should be
      labeled where. Meridians run north-south, and the letters `"N"`
      and `"S"` indicate that they should be labeled on their north or
      south end points, respectively. Parallels run east-west, and the
      letters `"E"` and `"W"` indicate that they should be labeled on
      their east or west end points, respectively. Thus,
      `label_graticule = "SW"` would label meridians at their south end
      and parallels at their west end, whereas `label_graticule = "EW"`
      would label parallels at both ends and meridians not at all.
      Because meridians and parallels can in general intersect with any
      side of the plot panel, for any choice of `label_graticule` labels
      are not guaranteed to reside on only one particular side of the
      plot panel. Also, `label_graticule` can cause labeling artifacts,
      in particular if a graticule line coincides with the edge of the
      plot panel. In such circumstances, `label_axes` will generally
      yield better results and should be used instead.

      This parameter can be used alone or in combination with
      `label_axes`.

  `label_axes`

  :   Character vector or named list of character values specifying
      which graticule lines (meridians or parallels) should be labeled
      on which side of the plot. Meridians are indicated by `"E"` (for
      East) and parallels by `"N"` (for North). Default is `"--EN"`,
      which specifies (clockwise from the top) no labels on the top,
      none on the right, meridians on the bottom, and parallels on the
      left. Alternatively, this setting could have been specified with
      `list(bottom = "E", left = "N")`.

      This parameter can be used alone or in combination with
      `label_graticule`.

  `lims_method`

  :   Method specifying how scale limits are converted into limits on
      the plot region. Has no effect when `default_crs = NULL`. For a
      very non-linear CRS (e.g., a perspective centered around the North
      pole), the available methods yield widely differing results, and
      you may want to try various options. Methods currently implemented
      include `"cross"` (the default), `"box"`, `"orthogonal"`, and
      `"geometry_bbox"`. For method `"cross"`, limits along one
      direction (e.g., longitude) are applied at the midpoint of the
      other direction (e.g., latitude). This method avoids excessively
      large limits for rotated coordinate systems but means that
      sometimes limits need to be expanded a little further if extreme
      data points are to be included in the final plot region. By
      contrast, for method `"box"`, a box is generated out of the limits
      along both directions, and then limits in projected coordinates
      are chosen such that the entire box is visible. This method can
      yield plot regions that are too large. Finally, method
      `"orthogonal"` applies limits separately along each axis, and
      method `"geometry_bbox"` ignores all limit information except the
      bounding boxes of any objects in the `geometry` aesthetic.

  `ndiscr`

  :   Number of segments to use for discretising graticule lines; try
      increasing this number when graticules look incorrect.

  `default`

  :   Is this the default coordinate system? If `FALSE` (the default),
      then replacing this coordinate system with another one creates a
      message alerting the user that the coordinate system is being
      replaced. If `TRUE`, that warning is suppressed.

  `clip`

  :   Should drawing be clipped to the extent of the plot panel? A
      setting of `"on"` (the default) means yes, and a setting of
      `"off"` means no. In most cases, the default of `"on"` should not
      be changed, as setting `clip = "off"` can cause unexpected
      results. It allows drawing of data points anywhere on the plot,
      including in the plot margins. If limits are set via `xlim` and
      `ylim` and some data points fall outside those limits, then those
      data points may show up in places such as the axes, the legend,
      the plot title, or the plot margins.

## Value

a `ggplot2` plot

## See also

the original
[`openair::trajPlot()`](https://openair-project.github.io/openair/reference/trajPlot.html)

[`trajMap()`](https://davidcarslaw.github.io/openairmaps/reference/trajMap.md)
for the interactive `leaflet` equivalent of `trajMapStatic()`

Other static trajectory maps:
[`trajLevelMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/trajLevelMapStatic.md)

## Examples

``` r
if (FALSE) { # \dontrun{
# colour by height
trajMapStatic(traj_data) +
  ggplot2::scale_color_gradientn(colors = openair::openColours())

# colour by PM10, log transform scale
trajMapStatic(traj_data, colour = "pm10") +
  ggplot2::scale_color_viridis_c(trans = "log10") +
  ggplot2::labs(color = openair::quickText("PM10"))

# color by PM2.5, lat/lon projection
trajMapStatic(traj_data, colour = "pm2.5", crs = sf::st_crs(4326)) +
  ggplot2::scale_color_viridis_c(option = "turbo") +
  ggplot2::labs(color = openair::quickText("PM2.5"))
} # }
```
