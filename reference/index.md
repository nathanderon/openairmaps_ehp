# Package index

## Network Visualisation

Quickly visualise UK air quality networks.

- [`networkMap()`](https://davidcarslaw.github.io/openairmaps/reference/networkMap.md)
  : Create a leaflet map of air quality measurement network sites

- [`searchNetwork()`](https://davidcarslaw.github.io/openairmaps/reference/searchNetwork.md)
  :

  Geographically search the air quality networks made available by
  [`openair::importMeta()`](https://openair-project.github.io/openair/reference/importMeta.html)

## Directional Analysis

### Data

Example data to use with directional mapping functions.

- [`polar_data`](https://davidcarslaw.github.io/openairmaps/reference/polar_data.md)
  : Example data for polar mapping functions

### Interactive

Create HTML `leaflet` maps with polar plot markers.

- [`annulusMap()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMap.md)
  : Polar annulus plots on interactive leaflet maps
- [`diffMap()`](https://davidcarslaw.github.io/openairmaps/reference/diffMap.md)
  : Bivariate polar plots on interactive leaflet maps
- [`freqMap()`](https://davidcarslaw.github.io/openairmaps/reference/freqMap.md)
  : Polar frequency plots on interactive leaflet maps
- [`percentileMap()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMap.md)
  : Percentile roses on interactive leaflet maps
- [`polarMap()`](https://davidcarslaw.github.io/openairmaps/reference/polarMap.md)
  : Bivariate polar plots on interactive leaflet maps
- [`pollroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMap.md)
  : Pollution rose plots on interactive leaflet maps
- [`windroseMap()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMap.md)
  : Wind rose plots on interactive leaflet maps
- [`addPolarMarkers()`](https://davidcarslaw.github.io/openairmaps/reference/addPolarMarkers.md)
  [`addPolarDiffMarkers()`](https://davidcarslaw.github.io/openairmaps/reference/addPolarMarkers.md)
  : Add polar markers to leaflet map

### Static

Create static `ggmap` maps with polar plot markers.

- [`annulusMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/annulusMapStatic.md)
  : Bivariate polar plots on a static ggmap
- [`diffMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/diffMapStatic.md)
  : Bivariate polar plots on a static ggmap
- [`freqMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/freqMapStatic.md)
  : Polar frequency plots on a static ggmap
- [`percentileMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/percentileMapStatic.md)
  : Percentile roses on a static ggmap
- [`polarMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/polarMapStatic.md)
  : Bivariate polar plots on a static ggmap
- [`pollroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/pollroseMapStatic.md)
  : Percentile roses on a static ggmap
- [`windroseMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/windroseMapStatic.md)
  : Wind rose plots on a static ggmap

## Trajectory Analysis

### Data

Example data to use witjh trajectory mapping functions.

- [`traj_data`](https://davidcarslaw.github.io/openairmaps/reference/traj_data.md)
  : Example data for trajectory mapping functions

### Interactive

Create HTML `leaflet` maps of HYSPLIT trajectories.

- [`trajLevelMap()`](https://davidcarslaw.github.io/openairmaps/reference/trajLevelMap.md)
  :

  Trajectory level plots in `leaflet`

- [`trajMap()`](https://davidcarslaw.github.io/openairmaps/reference/trajMap.md)
  :

  Trajectory line plots in `leaflet`

- [`addTrajPaths()`](https://davidcarslaw.github.io/openairmaps/reference/addTrajPaths.md)
  : Add trajectory paths to leaflet map

### Static

Create static `ggplot2` maps of HYSPLIT trajectories.

- [`trajLevelMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/trajLevelMapStatic.md)
  **\[experimental\]** :

  Trajectory level plots in `ggplot2`

- [`trajMapStatic()`](https://davidcarslaw.github.io/openairmaps/reference/trajMapStatic.md)
  **\[experimental\]** :

  Trajectory line plots in `ggplot2`

## Utilities

Tools to assist other openairmaps functions.

- [`buildPopup()`](https://davidcarslaw.github.io/openairmaps/reference/buildPopup.md)
  : Build a Complex Popup for a Leaflet Map
- [`quickTextHTML()`](https://davidcarslaw.github.io/openairmaps/reference/quickTextHTML.md)
  : Automatic text formatting for openairmaps
- [`convertPostcode()`](https://davidcarslaw.github.io/openairmaps/reference/convertPostcode.md)
  : Convert a UK postcode to a latitude/longitude pair
