# WordPress synchronisation
Some pages and widgets on wordpress are to be generated programmatically from data stored in DCMS. For example, the Team Dyalog grid/list and individual `/team-dyalog/` pages are generated in this way.

## How DCMS data becomes a WordPress Page
We *push* data up to wordpress via its REST API, to populate some fields in a JetEngine Custom Post Type (CPT). 

![JetEngine Post Types from WordPress menu](Pasted image 20220826111000.png)

There is a Template for each of these page types.

![Templates from WordPress menu](Pasted image 20220826105244.png)

The template has conditional display for a JetEngine Post Type. Click on the arrow next to the **PUBLISH/UPDATE** button and click **Display Conditions**. Choose **INCLUDE** the Post Type (e.g. Team Dyalog Member) and the custom fields should become accessible from within. 

![Template Display Conditions in Elementor](Pasted image 20220826111045.png)

We can now relate information for that specific page / DCMS API record. 

We will only use the Title (e.g. team member name) and `dcms_id` (ID into the DCMS backend database table, e.g. the `person` table).

The majority of the dynamic data is actually pulled from the DCMS API using a JetEngine REST API Endpoint and Custom Query.

<div class="inline-img">
    <img alt="JetEngine dashboard REST API Endpoints" src="../Pasted image 20220826173203.png" />
    <img alt="JetEngine Query Builder from WordPress menu" src="../Pasted image 20220826173228.png" />
</div>

!!!Note "Why don't we just use the Custom Post Type Meta Fields in the Dynamic Tags in Elementor?"
    Although it is possible to push all of the fields from DCMS database to the Custom Post Type and use them directly within the Template as Dynamic Tags → Custom Fields, there is an extra step to adding a new field. We would have to add that meta field to the Custom Post Type and then add it to the Listing.

A JetEngine Listing is where the arrangement and styling for displaying the data on the website occurs.

![JetEngine Listings from WordPress menu](Pasted image 20220826173358.png)

This Listing can then be included in the Template as an Elementor Listing Grid widget.

!!!Note "Why can't I see anything in the Listing or Template?"
    The JetEngine Listings item for a REST API endpoint has an example visible in Elementor when editing, whereas a Listings item for a Query Builder query does not.

    For a single page, it is best to create a static template, and then copy the structure into a Query Builder Listings item where the widgets content can be replaced with Dynamic Tags → Query Builder items.

    For a page showing a list or grid of items, you might get good results using a Listing from a REST API Endpoint.

## Push data from DCMS to WordPress via REST
!!!Warning "ISSUE"
	For some reason, I'm not able to update meta fields in JetEngine Custom Post Type posts using the WordPress API. The `title` (name) and `slug` (URL slug) fields are updated, but not the meta dcms_id field.

	It may be possible to work around this by changing the ID in the database

	For now, we are going to use the WordPress mechanisms to handle most of the data. DCMS will be developed primarily to drive the new Dyalog.TV video library.

A custom post type contains the ID into the relevant SQL table. For example, the `team-dyalog` CPT contains fields:

- `title` (The person's chosen name), 
- `slug` (URL slug for this person's page)

and a meta field

- `dcms_id` (ID into the DCMS `person` endpoint/table)

This data is pushed to WordPress via the REST API in the `#.DCMS.wp_` namespace. The data is simply supplied as JSON.

The `/refresh` endpoint can be used to trigger a rebuild of the API data and an update of wordpress CPT Posts. 

### Authentication
A WordPress user `DCMS` has permission to edit posts. You can create a password (`#.GLOBAL.api.wordpress_token` in the [secrets](secrets.md)) from Wordpress via **Users → Profile → Application Password**.

