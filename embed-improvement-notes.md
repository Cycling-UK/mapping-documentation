1. [Route map](route-content-type.md)
2. [Route landing page maps](route-landing-page-content-type.md)
3. [Paragraphs](paragraph-embeds.md)
4. [Modules](relevant-modules.md)
5. [Listings](listing-pages.md)
6. [Journey planner](journey-planner.md)
7. [The Drupal APIs](api.md)
8. [CAN map paragraph type](can.md)
9. [The embeds could use improvements](embed-improvement-notes.md)

# Notes on doing a better map embed

Your current approach to embedding JavaScript in your Drupal 10 Twig template looks functional, but there are better practices you can implement. Let's explore the recommended methods for including JavaScript in Drupal 10.

## Recommended Approaches
### 1. Use Drupal's Asset Library System
The best practice in Drupal 10 is to use the asset library system rather than inline JavaScript. This gives you better caching, performance, and maintainability.

```// In your_module.libraries.yml
your_map_library:
  js:
    js/map-initializer.js: {}
  dependencies:
    - core/drupal
    - core/jquery
    - core/once
```
### Then in your JS file:

```
(function (Drupal) {
  'use strict';

  Drupal.behaviors.yourMapBehavior = {
    attach: function (context, settings) {
      once('map-init', '[id^="map-"]', context).forEach(function (element) {
        const mapId = element.id;
        const routeId = element.getAttribute('data-route-id');
        const showPOIs = element.getAttribute('data-show-pois') === 'true';

        initialiseMap(mapId, {
          preset: 'route',
          routeID: routeId,
          showSearch: false,
          showPOIs: showPOIs,
          sidepanel: false,
          filterPanel: false,
          scrollproof: true
        });
      });
    }
  };
})(Drupal);
```
### In your Twig template:
```
{{ attach_library('your_module/your_map_library') }}

<div id="map-{{ routeUUID|trim }}"
     data-route-id="{{ routeUUID|render|trim }}"
     data-show-pois="{{ 'On' in content.field_display_pois.0 ? 'true' : 'false' }}">
</div>
```

### 2. Use Drupal's Twig drupal_add_js or Pass Settings via `drupalSettings`
If you need to keep your JavaScript more dynamic:

```
// In your module or theme
$routeUUID = $variables['routeUUID'];
$showPOIs = in_array('On', $variables['content']['field_display_pois'][0]);

$variables['#attached']['drupalSettings']['yourMapModule']['maps'][$routeUUID] = [
  'preset' => 'route',
  'routeID' => $routeUUID,
  'showSearch' => false,
  'showPOIs' => $showPOIs,
  'sidepanel' => false,
  'filterPanel' => false,
  'scrollproof' => true
];

$variables['#attached']['library'][] = 'your_module/your_map_library';
```

Then in your JS:

```
(function (Drupal) {
  'use strict';

  Drupal.behaviors.yourMapBehavior = {
    attach: function (context, settings) {
      if (settings.yourMapModule && settings.yourMapModule.maps) {
        Object.keys(settings.yourMapModule.maps).forEach(function(routeId) {
          const mapId = 'map-' + routeId;
          const mapConfig = settings.yourMapModule.maps[routeId];

          once('map-init', '#' + mapId, context).forEach(function(element) {
            initialiseMap(mapId, mapConfig);
          });
        });
      }
    }
  };
})(Drupal);
```
### 3. If You Must Use Inline JavaScript (Not Recommended)
If you absolutely need to keep your inline approach, you can make it cleaner:
**(this is currently what's on the route content type)**
```
<div id="map-{{ routeUUID|trim }}"></div>

<script>
  (function() {
    window.addEventListener("load", function() {
      initialiseMap('map-{{ routeUUID|trim }}', {
        preset: 'route',
        routeID: '{{ routeUUID|render|trim }}',
        showSearch: false,
        showPOIs: {{ 'On' in content.field_display_pois.0 ? 'true' : 'false' }},
        sidepanel: false,
        filterPanel: false,
        scrollproof: true
      });
    });
  })();
</script>
```

### Best Practices Summary

1. **Use Drupal's asset library system** whenever possible
2. **Pass dynamic values via data attributes or drupalSettings**
3. **Follow Drupal's JavaScript coding standards**
4. **Use Drupal.behaviors** for proper attachment and detachment
5. **Use the 'once' utility** to prevent duplicate initialization

The first approach with the asset library system is the most Drupal-friendly and maintainable solution for your map embedding needs.
