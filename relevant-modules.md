1. [Route map](route-content-type.md)
2. [Route landing page maps](route-landing-page-content-type.md)
3. [Paragraphs](paragraph-embeds.md)
4. [Modules](relevant-modules.md)
5. [Listings](listing-pages.md)
6. [Journey planner](journey-planner.md)
7. [The Drupal APIs](api.md)
8. [CAN map paragraph type](can.md)

# Modules
Anthony’s modules and what they do

## cyclinguk_gpxdata (in custom directory)
This module simply sends data to Richard's world for quick analysis which is then returned to fill in some of the route data fields on the **route content type**.

## nearme (in contrib directory)
Queries Richard's database to produce views with contain an embedded map and attached views list of returned data.  Has custom display format of Richards map to use in views.

Also does the work to get gpx data into Richard's database on saving of route content type data, so in theory maps should appear relatively quickly following content save.

## Configuration
There is a configuration page for the Cycling UK Near Me module.

[cyclinguk.org/admin/config/services/cyclinguk_nearme](https://www.cyclinguk.org/admin/config/services/cyclinguk_nearme)

It has settings to handle get and push for live and test mapping databases. Also has an off button to not push.

#### configuration page:
<img src="./assets/map-doc-near-me-config.png" alt="configuration page" style="padding: 10px;"/>


## Sacha’s modules and what they do
The modules I put together related to mapping are to introduce the required libraries. That means the modules make available the necessary javascript and css for the maps to function and look ok. The main one is the **cycle_travel_mapping** module. I makes available the css, which as seen below we are hosting locally, and the variety of remote javascript:

```cycle_travel_mapping:
  css:
    base:
      css/maplibre-gl.css: { minified: true }
      css/autocomplete-theme-classic.css: { minified: true }

    layout:
      css/prototype.css: {}
      css/responsive.css: {}

  js:
    https://cycling-uk-d9.cycle.travel/maps/third_party/maplibre-gl.js: { type: external}
    https://cycling-uk-d9.cycle.travel/maps/third_party/autocomplete.js: { type: external }
    https://cycling-uk-d9.cycle.travel/maps/third_party/uPlot.js: { type: external}
    https://kit.fontawesome.com/9a2e8d8cca.js: { type: external, attributes: { crossorigin: anonymous } }
    https://cycling-uk-d9.cycle.travel/maps/cycling_uk/scripts/app.js: { attributes: { type: module } }
    https://cycling-uk-d9.cycle.travel/maps/cycling_uk/scripts/simple_map.js: { attributes: { type: module } }
```



  - **cycle_travel_mapping** ```{{ attach_library('cycle_travel_mapping/cycle_travel_mapping') }}```
  - **rastermap_embed** ```{{ attach_library('rastermap_embed/rastermap') }}```
  - **map_embed** (I think redundant, replaced by cycle_travel_mapping, need to check)
  - **simplemap_embed**
