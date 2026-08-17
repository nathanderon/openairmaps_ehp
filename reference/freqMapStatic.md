# Polar frequency plots on a static ggmap

`freqMapStatic()` creates a `ggplot2` map using polar frequency plots as
markers. As this function returns a `ggplot2` object, further
customisation can be achieved using functions like
[`ggplot2::theme()`](https://ggplot2.tidyverse.org/reference/theme.html)
and
[`ggplot2::guides()`](https://ggplot2.tidyverse.org/reference/guides.html).

## Usage

``` r
freqMapStatic(
  data,
  pollutant = NULL,
  ggmap,
  statistic = "mean",
  breaks = "free",
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
[`openair::polarFreq()`](https://openair-project.github.io/openair/reference/polarFreq.html)

[`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md)
for the interactive `leaflet` equivalent of `freqMapStatic()`

Other static directional analysis maps:
[`annulusMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMapStatic.md),
[`diffMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/diffMapStatic.md),
[`percentileMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMapStatic.md),
[`polarMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/polarMapStatic.md),
[`pollroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMapStatic.md),
[`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
