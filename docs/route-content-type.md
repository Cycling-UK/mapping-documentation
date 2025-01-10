## Maps on Route content type
<img src="https://www.cyclinguk.org/sites/default/files/2025/01/map-doc-route.png" alt="a route page" width="200" style="float: right; padding: 10px;"/>

### What is it

The route content type is the primary place where Cycling UK routes are displayed on a map. `/admin/structure/types/manage/route/fields`

### How to create a map on a route page

Uploading the gpx file is a requirement of a route page. There are a few steps for this, which sit within "The route" tab on the page edit form.

1. Click the `Choose file` button to browse to find your gpx file and upload it.
2. After the file is uploaded click the yellow button `PROCESS GPX FOR ROUTE INFORMATION`. This will send the gpx data to the mapping server for analysis and with any luck return some data to populate the route meta data fields below. The meta data fields can also be updated manually if necessary. 

<img src="https://www.cyclinguk.org/sites/default/files/2025/01/map-doc-gpx-upload.png" alt="gpx upload" style="padding: 10px;"/>

### The code behind the scenes

This relies on the Richard Fairhurst's mapping service which at a codebase seperate to the Drupal website.  On the website we have import a series of tools and have built a template to embed the map created on Richard's server into the Drupal webpage. We have built a user interface to interact with the embed code present in the template.
