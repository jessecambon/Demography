tidygeocoder demo
================

Geocode some addresses in DC with tidygeocoder

``` r
library(dplyr)
library(tidygeocoder)

# Make a tibble of addresses in DC
dc_addresses <- tribble( ~name,~addr,
       "White House", "1600 Pennsylvania Ave Washington, DC",
       "National Academy of Sciences", "2101 Constitution Ave NW, Washington, DC 20418",
       "Department of Justice", "950 Pennsylvania Ave NW, Washington, DC 20530",
       "Supreme Court", "1 1st St NE, Washington, DC 20543",
       "Washington Monument", "2 15th St NW, Washington, DC 20024"
                           )

# Geocode the addresses with the US Census geocoder
coordinates <- dc_addresses %>%
  tidygeocoder::geocode(addr)
```

References:

  - <https://www.linkedin.com/pulse/plot-over-openstreetmap-ggplot2-abel-tortosa-andreu/>
  - <https://www.openstreetmap.org/export#map=14/38.8982/-77.0251>

Pull OSM map data for DC. Use coordinates pulled from the
openstreemap.org GUI (click export button)

``` r
library(OpenStreetMap)

# Get DC Map
dc_map <- openmap( c(38.905,-77.05),c(38.885,-77.00))
dc_map.latlng <- openproj(dc_map)
```

Plot our points on our DC map

``` r
library(ggplot2)
library(ggrepel)
  
autoplot(dc_map.latlng) +
  theme_bw() +
  theme(title = element_blank(),
        line=element_blank()) +
  geom_point(data=coordinates, aes(x=long, y=lat), color="navy", size=4, alpha=1) +
  geom_label_repel(data=coordinates,
        aes(label=name,x=long, y=lat),show.legend=F,box.padding=.5)
```

![](tidygeocoder-demo_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
