# Percentile roses on a static ggmap

`percentileMapStatic()` creates a `ggplot2` map using percentile roses
as markers. As this function returns a `ggplot2` object, further
customisation can be achieved using functions like
[`ggplot2::theme()`](https://ggplot2.tidyverse.org/reference/theme.html)
and
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html).

## Usage

``` r
percentileMapStatic(
  data,
  pollutant = NULL,
  ggmap,
  percentile = c(25, 50, 75, 90, 95),
  intervals = "fixed",
  latitude = NULL,
  longitude = NULL,
  facet = NULL,
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

- facet:

  Used for splitting the input data into different panels, passed to the
  `type` argument of
  [`openair::cutData()`](https://openair-project.github.io/openair/reference/cutData.html).
  `facet` cannot be used if multiple `pollutant` columns have been
  provided.

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
  [`openair::percentileRose`](https://openair-project.github.io/openair/reference/percentileRose.html)

  `wd`

  :   Name of wind direction field.

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

      Type can be up length two e.g. `type = c("season", "weekday")`
      will produce a 2x2 plot split by season and day of the week. Note,
      when two types are provided the first forms the columns and the
      second the rows.

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
[`openair::percentileRose()`](https://openair-project.github.io/openair/reference/percentileRose.html)

[`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md)
for the interactive `leaflet` equivalent of `percentileMapStatic()`

Other static directional analysis maps:
[`annulusMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMapStatic.md),
[`diffMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/diffMapStatic.md),
[`freqMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/freqMapStatic.md),
[`polarMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/polarMapStatic.md),
[`pollroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMapStatic.md),
[`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
