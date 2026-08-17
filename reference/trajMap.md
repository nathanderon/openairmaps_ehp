# Trajectory line plots in `leaflet`

This function plots back trajectories on a `leaflet` map. This function
requires that data are imported using the
[`openair::importTraj()`](https://openair-project.github.io/openair/reference/importTraj.html)
function. Options are provided to colour the individual trajectories
(e.g., by pollutant concentrations) or create "layer control" menus to
show/hide different layers.

## Usage

``` r
trajMap(
  data,
  longitude = "lon",
  latitude = "lat",
  colour,
  control = "default",
  cols = "default",
  alpha = 0.5,
  npoints = 12,
  provider = "OpenStreetMap",
  collapse.control = FALSE
)
```

## Arguments

- data:

  Data frame, the result of importing a trajectory file using
  [`openair::importTraj()`](https://openair-project.github.io/openair/reference/importTraj.html).

- latitude, longitude:

  The decimal latitude/longitude.

- colour:

  Column to be used for colouring each trajectory. This column may be
  numeric, character or factor. This will commonly be a pollutant
  concentration which has been joined (e.g., by
  [`dplyr::left_join()`](https://dplyr.tidyverse.org/reference/mutate-joins.html))
  to the trajectory data by "date".

- control:

  Used for splitting the trajectories into different groups which can be
  selected between using a "layer control" menu. Passed to
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).

- cols:

  Colours to be used for plotting. Options include "default",
  "increment", "heat", "turbo" and `RColorBrewer` colours — see the
  [`openair::openColours()`](https://openair-project.github.io/openair/reference/openColours.html)
  function for more details. For user defined the user can supply a list
  of colour names recognised by R (type
  [`grDevices::colours()`](https://rdrr.io/r/grDevices/colors.html) to
  see the full list). An example would be
  `cols = c("yellow", "green", "blue")`. If the `"colour"` argument was
  not used, a single colour can be named which will be used consistently
  for all lines/points (e.g., `cols = "red"`).

- alpha:

  Opacity of lines/points. Must be between `0` and `1`.

- npoints:

  A dot is placed every `npoints` along each full trajectory. For hourly
  back trajectories points are plotted every `npoints` hours. This helps
  to understand where the air masses were at particular times and get a
  feel for the speed of the air (points closer together correspond to
  slower moving air masses). Defaults to `12`.

- provider:

  The base map to be used. See
  <http://leaflet-extras.github.io/leaflet-providers/preview/> for a
  list of all base maps that can be used.

- collapse.control:

  Should the "layer control" interface be collapsed? Defaults to
  `FALSE`.

## Value

A leaflet object.

## See also

the original
[`openair::trajPlot()`](https://openair-project.github.io/openair/reference/trajPlot.html)

[`trajMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/trajMapStatic.md)
for the static `ggplot2` equivalent of `trajMap()`

Other interactive trajectory maps:
[`trajLevelMap()`](https://davidcarslaw.github.io/openairmaps/reference/trajLevelMap.md)

## Examples

``` r
if (FALSE) { # \dontrun{
trajMap(traj_data, colour = "nox")
} # }
```
