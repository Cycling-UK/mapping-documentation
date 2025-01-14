1. [Route map](route-content-type.md)
2. [Route landing page maps](route-landing-page-content-type.md)
3. [Paragraphs](paragraph-embeds.md)
4. [Modules](relevant-modules.md)
5. [Listings](listing-pages.md)
6. [Journey planner](journey-planner.md)
7. [The Drupal APIs](API.md)

# APIs to expose data to Richard F's service
##
The data required for mapping is made available to Richard's service with a few APIs, in json format.

The APIs have been built using views. The APIs take in data from the variety of relevant content types and
repackage the data in json format on specified urls. There are several APIs:

### Built in the view at /admin/structure/views/view/endpoints
- The primary API for content is [www.cyclinguk.org/API/v1](https://www.cyclinguk.org/API/v1)
    - This API holds the content types **Route**, **Post page**, **Group (workaround)**, and **Point of interest**.
- There is an Event API [www.cyclinguk.org/API/v1/event](https://www.cyclinguk.org/API/v1/event)
    - This API holds the **Event** content type.
    - This is separate because it adding it to the main API causes problems, it would cause the main API to crash.
### Built in the view at /admin/structure/views/view/endpoint_groups 
Groups as usual are problematic. I cannot build a view that holds both Content and Groups because they are different types of entities in Drupal. So to get Group entity content into the main content API we've implemented an Ian workaround:

A Group API and Feeds are used to create standard Content type content using a Content type named Group (workaround).
- I built an API of actual Group entity content: [www.cyclinguk.org/API/v1/group-workaround](https://www.cyclinguk.org/API/v1/group-workaround) The view for this is at /admin/structure/views/view/endpoint_groups/edit/rest_export_1.
- That Group API is used as the feed url for a Groups workaround feed I built. The Feed imports every day into the Content type named Group (workaround), maintaining a list of groups with some essential group data as the standard Group (workaround) content type content. This then allows me to integrate Groups into the main API at [www.cyclinguk.org/API/v1](https://www.cyclinguk.org/API/v1).
- In my view this is a terrible idea. 