* Mapping
    1. [Route map](route-content-type.md)
    2. [Route landing page maps](route-landing-page-content-type.md)
    3. [Paragraphs](paragraph-embeds.md)
    4. [Modules](relevant-modules.md)
    5. [Listings](listing-pages.md)
    6. [Journey planner](journey-planner.md)
    7. [The Drupal APIs](api.md)
    8. [CAN map paragraph type](can.md)
    9. [The embeds could use improvements](embed-improvement-notes.md)
    10. [The mapping dev server](devserver.md)
    11. [Mapping - tag map paragraph type](tagmap.md)
* FundraiseUp
    1. [FundraiseUp wide image donate paragraph type](fundraiseup-wide.md)



# Tag map
## Tag map no search paragraph type
We can embed a map that displays content by tag.

We've been prompted to come up with this by the prospect of TfL Cycle Sundays and by Sophie, who wants a map of her current 'Glow ride' campaign.

The solution is to use tags as the mechanism.  The paragraph type named __Tag map no search__ is essentially just a reference to a views block. That view is a 'near me' style view which embeds a Richard Fairhurst map. The view has a filter on it which finds the tag used on the page it is embedded on and uses that tag for the filter - to display only content which has that tag.

__Tag map no search__ is currently only enabled on the landing page content type.

### The process to use __Tag map no search__ is:

1. Add the Tag map no search paragraph to your content. On the paragraph type nothing is required other than adding the paragraph type. It does have title and text fields which can be used if desired.
2. Tag the page the  __Tag map no search__ is embedded on. The paragraph will take that tag and use it to find the content to display in the map.


Currently the view that drives this is set to display Routes, Points of interest, Events, and Groups content by tag, but we aren't using the poi content type yet and so far tags haven't been used much on the other content types. And on the Event content type the tags field has only recently been added. It has mostly been tested with Event and Route type content.
