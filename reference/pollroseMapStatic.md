# Percentile roses on a static ggmap

`pollroseMapStatic()` creates a `ggplot2` map using percentile roses as
markers. As this function returns a `ggplot2` object, further
customisation can be achieved using functions like
[`ggplot2::theme()`](https://ggplot2.tidyverse.org/reference/theme.html)
and
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html).

## Usage

``` r
pollroseMapStatic(
  data,
  pollutant = NULL,
  ggmap,
  statistic = "prop.count",
  breaks = NULL,
  facet = NULL,
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

- facet:

  Used for splitting the input data into different panels, passed to the
  `type` argument of
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).
  `facet` cannot be used if multiple `pollutant` columns have been
  provided.

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
[`openair::pollutionRose()`](https://openair-project.github.io/openair/reference/pollutionRose.html)

[`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md)
for the interactive `leaflet` equivalent of `pollroseMapStatic()`

Other static directional analysis maps:
[`annulusMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMapStatic.md),
[`diffMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/diffMapStatic.md),
[`freqMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/freqMapStatic.md),
[`percentileMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMapStatic.md),
[`polarMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/polarMapStatic.md),
[`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
