---
layout: doc
description: How to build a simple microapp using a public RSS feed
---
# Title
Building a simple Citrix microapp that shows blog posts from a Wordpress RSS feed
## Contributors

**Author:** [Phil Wiffen](https://twitter.com/phil_wiffen)


---
![screenshot showing how the RSS microapp would look](/en-us/tech-zone/design/media/deployment-guides_microapps-rss_example-screenshot.png)

## Scope

This post will cover how to setup a simple microapp that anyone with access to the Citrix microapps service can build, using public URLs, with no authentication requirements. We'll be using the RSS integration to talk to a Wordpress RSS feed and build microapps from that.

## Why bother?

I've spent the last few months learning about microapps, and implementing them into Citrix Engineering's pre-release Workspace environments - and honestly, I wish I had a guide like this to follow when I first started. So here we are :)

My hope is that following this will help you get familiar with the concepts, before you dive into the heavier stuff, like accessing internal APIs, or figuring out the details around Oauth 2.0 authentication.

By following this post you'll learn:

*   How to add an Integration and a microapp
*   How to make changes to how a microapp displays data

## What are we building?

We'll build a Citrix Blog posts microapp, which uses the [RSS Out-of-the-box integration](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-rss.html) to connect to the Citrix Blogs RSS feed, which happens to be in a Wordpress format and:



*   Notifies Workspace when there's a new blog post
*   Enables colleagues to view blog posts from Workspace
*   Allows colleagues to view a list of blog posts in a searchable table


## Before you start

This guide assumes that you already have access to the Microapps service in your Citrix Cloud account.

Familiarise yourself with the following microapps concepts by [reading the documentation](https://docs.citrix.com/en-us/citrix-microapps.html#terminology?)

*   Integration
*   Microapp
*   Notification
*   Page
*   Action

I'll be using the above terms liberally, so knowing what they mean will help.

## Let's build it!

### We're going to:

1.  Add an RSS Integration to sync Citrix Blog posts
2.  Change the name of the microapp so it's more intuitively named in Workspace Actions
3.  Change the sort order of Blog posts to: Date, Descending
4.  Change the Description field of the Item Detail page to show HTML content for prettier viewing
5.  Set the Title of the blog post to be in the header of the page
6.  Add a "View Post" button, that goes to the Blog Post online
7.  Setup the Synchronization Schedule
8.  Add Subscribers to the microapp (so they can see Notifications and Actions)

### Add the Integration

Go to the Microapps Admin page, and choose "Add New Integration"



<!-- wp:image {"id":1589} -->
<figure class="wp-block-image">![](2019-12-16-22_00_30-Microapps-Admin.png)</figure>
<!-- /wp:image -->


Choose the RSS Integration


<!-- wp:image {"id":1598} -->
<figure class="wp-block-image">![](2019-12-16-22_12_50-Microapps-Admin.png)</figure>
<!-- /wp:image -->

Call the Integration a name, such as  Citrix Blogs (RSS), and enter the URL to the Wordpress RSS feed. In this example, for Citrix Blogs, we're using http://feeds.feedblitz.com/citrix&x=1 


<!-- wp:image {"id":1591} -->
<figure class="wp-block-image">![](2019-12-16-22_04_39-Microapps-Admin.png)</figure>
<!-- /wp:image -->


You'll then be taken to the Microapps list of integrations, and you'll see the new integration. Inside, it'll already have a microapp configured called "Items", and you'll see it has Synchronized


<!-- wp:image {"id":1596} -->
<figure class="wp-block-image">![](2019-12-16-22_08_53-Microapps-Admin-1024x189.png)</figure>
<!-- /wp:image -->

Because the RSS Integration is deliberately generic - we need to make a number of tweaks to make the Blog microapp nicer to use and look at.

### Change the name of the Microapp

We change the name of the microapp, because this is how it appears in the Actions pane in Workspace. If we keep it as "Items", that's not particularly user friendly. In our environments, we rename this microapp to Citrix Blogs, so people will know what they get when they click the Action. 

To change the Microapp name:

1.  Click on "Items"
2.  Go to Properties
3.  Change the App Name, and the App Description
4.
5.  Click Save

<!-- wp:image {"id":1602,"width":440,"height":542} -->
<figure class="wp-block-image is-resized">![](2019-12-16-22_18_04-Microapps-Admin-2.png)</figure>
<!-- /wp:image -->
Here's a comparison of how they'd look in Workspace. I much prefer it to say Citrix Blogs. rather than Items :)

<!-- wp:image {"id":1604} -->
<figure class="wp-block-image">![](2019-12-16-22_35_40-Citrix-Workspace.png)</figure>
<!-- /wp:image -->

### Change the sort order of Blog posts to Date, Descending 


Out of the box, the Items table is not sorted by Date. Let's fix that so it's suitable for a Blog post list that shows the latest posts at the top of the table.

In the Citrix Blogs microapp, click on Pages, then click on Items

<!-- wp:image {"id":1611} -->
<figure class="wp-block-image">![](2019-12-16-22_47_27-Microapps-Admin-3-1024x312.png)</figure>
<!-- /wp:image -->

Click the Table, so that it's highlighted (a blue x will appear in the top-right of the table). Then click Set Order

<!-- wp:image {"id":1627} -->
<figure class="wp-block-image">![](2019-12-16-23_16_53-Microapps-Admin-1-1024x442.png)</figure>
<!-- /wp:image -->


1.  Set the Order by to: items, published_at, Descending.
2.  Click Save
3.

<!-- wp:image {"id":1613} -->
<figure class="wp-block-image">![](2019-12-16-22_51_30-Microapps-Admin.png)</figure>
<!-- /wp:image -->


Bonus points: Change the table Label from Items to Blog Posts. Again, this just makes it look nicer.


<!-- wp:image {"id":1614} -->
<figure class="wp-block-image">![](2019-12-16-23_02_47-Microapps-Admin.png)</figure>
<!-- /wp:image -->

### Displaying HTML content from the blog posts


Just FYI: This HTML component is in the product at GA because I had issues with showing blog content in the microapp page, and the Product Management team did amazing work to accelerate their plans for this functionality. It sounds small, but it's just one tangible example of what I (and my team) do inside Citrix - we use the product, we feedback, we help make things better

Out of the box, the RSS Integration shows raw text. It works really well for many kinds of feeds/data, but for some RSS feeds, the fields have HTML embedded in them.


Out of the box, it looks like this:

<!-- wp:image {"id":1636} -->
<figure class="wp-block-image">![](2019-12-17-21_24_46-Microapps-Admin-5-588x1024.png)</figure>
<!-- /wp:image -->

And we're going to tidy it up so it looks like this:

<!-- wp:image {"id":1701} -->
<figure class="wp-block-image">![](2019-12-17-22_36_03-Microapps-Admin.png)</figure>
<!-- /wp:image -->

### Add the blog title to the header of the Page

Set the Blog Title as the page title and remove the existing title. This makes the title look much nicer:

*   Go to Pages, then click on Item Detail
*   Click the Back button in the Builder viewer
*   In the right-hand pane, set the Title Template from blank, to {{title}}

<!-- wp:image {"id":1703} -->
<figure class="wp-block-image">![](2019-12-17-21_25_46-1024x265.png)</figure>
<!-- /wp:image -->


*   Click the Title text in the main body of the Builder viewer, and delete it (because the title is now shown at the top of the page)
*   This will now show the Blog post title in the Page header (and looks wayyyyyyy nicer)

Replace the Description default Test component, with the HTML Content component:


1.  Drag the HTML Content component so it goes above where the current Description text component is
2.

<!-- wp:image {"id":1649} -->
<figure class="wp-block-image">![](2019-12-17-21_33_41-Microapps-Admin-10-1024x630.png)</figure>
<!-- /wp:image -->

1.  Set the Label (you can make it blank - it's not required) and set the Data Table to "Items" and the Data column to "Description"
2.  Delete the other Description component, by clicking on it, then clicking the X in the top=right hand corner.P

<!-- wp:image {"id":1678} -->
<figure class="wp-block-image">![](2019-12-17-21_47_49-1024x375.png)</figure>
<!-- /wp:image -->



It'll then populate the HTML component and look much nicer!



<!-- wp:image {"id":1698} -->
<figure class="wp-block-image">![](2019-12-17-22_01_28-3-1024x580.png)</figure>
<!-- /wp:image -->


Personally, I remove the Categories Table, as it serves no purpose in this view


<!-- wp:image {"id":1697} -->
<figure class="wp-block-image">![](2019-12-17-22_06_53-Microapps-Admin-1.png)</figure>
<!-- /wp:image -->

## Add a button to link to the Blog Post

Finally, let's add a button to link to the Blog Post. This is a nice way to allow people to open the blog post if they want to read more than just the lede.


Drag the "Button" button to the bottom of the page:



<!-- wp:image {"id":1696} -->
<figure class="wp-block-image">![](2019-12-17-22_08_52-Microapps-Admin-1-1024x925.png)</figure>
<!-- /wp:image -->



Then, set the button up like so...



Change the Button Label from Button, to "View Post" (or whatever you'd like it to say :))


<!-- wp:image {"id":1699} -->
<figure class="wp-block-image">![](2019-12-17-22_34_52-Microapps-Admin.png)</figure>
<!-- /wp:image -->


Now we setup the link part.

Click Actions, on the right hand side:

<!-- wp:image {"id":1688} -->
<figure class="wp-block-image">![](2019-12-17-22_09_54-Microapps-Admin-1-1024x207.png)</figure>
<!-- /wp:image -->

Then on the Drop down, choose "Go to URL"

<!-- wp:image {"id":1685} -->
<figure class="wp-block-image">![](2019-12-17-22_10_38-Microapps-Admin.png)</figure>
<!-- /wp:image -->

Expand the Goto URL, then click on Insert Variable

<!-- wp:image {"id":1684} -->
<figure class="wp-block-image">![](2019-12-17-22_11_27-Microapps-Admin.png)</figure>
<!-- /wp:image -->

From the dropdown, choose "url" and click Insert:

<!-- wp:image {"id":1695} -->
<figure class="wp-block-image">![](2019-12-17-22_15_22-1.png)</figure>
<!-- /wp:image -->


You'll then see the field populated with {{url}}. This will insert the Blog Post's URL into the button, and will launch the site when clicked.

Preview the microapp to see your handiwork:

<!-- wp:image {"id":1704} -->
<figure class="wp-block-image">![](2019-12-17-22_44_05-Microapps-Admin.png)</figure>
<!-- /wp:image -->

Looks good!


<!-- wp:image {"id":1710} -->
<figure class="wp-block-image">![](2019-12-17-23_06_33-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:image {"id":1705} -->
<figure class="wp-block-image">![](2019-12-17-22_44_43-Microapps-Admin-1024x732.png)</figure>
<!-- /wp:image -->


### Set a Synchronization Schedule

Now we've made the microapp, with its pages, we should set a Synctonization Schedule.

The schedule is up to you and the feed you're pulling in. For Citrix Blogs, we set this to once every hour, which is a good balance between keeping things up to date, but not hitting the website too much with requests.

After a sync happens, if a new blog post entry is found a notification is sent to Subscribers to that microapp, and it appears in the Workspace feed (and, if enabled, a Push Notification to the person's device with Citrix Workspace app, too)

_Please note: The first time you sync - no Notifications will be generated. So when you look in your feed, there won't be any Notifications yet. This is because notifications are generated the **next** time a sync occurs **and **there's new blog posts. The first sync will simply load up the microapps cache. You'll need to wait for a new blog post to be posted into the RSS, and for a sync to happen, for a notification to be generated._

You set the Synchronization by going to the Microapp Integrations page, clicking the three dots next to the integration and choosing Synchronization

<!-- wp:image {"id":1706} -->
<figure class="wp-block-image">![](2019-12-17-22_47_32-Microapps-Admin-1024x165.png)</figure>
<!-- /wp:image -->

### Adding Subscribers to the microapp

The final step of this process: Giving people access to the microapp itself. Without being granted this, people won't see the microapp, or the notifications

To give people access, Click the three dots next to the microapp, and choose Subscriptions


<!-- wp:image {"id":1707} -->
<figure class="wp-block-image">![](2019-12-17-22_51_15-Microapps-Admin-1024x232.png)</figure>
<!-- /wp:image -->

In the example below, I'm showing that you can add Security Groups, as well as individual users. This can help you test, and gradually roll out the microapp in a live environment in phases, to help ensure quality and user acceptance.


<!-- wp:image {"id":1708} -->
<figure class="wp-block-image">![](2019-12-17-22_52_51-Microapps-Admin.png)</figure>
<!-- /wp:image -->

## Pat yourself on the back

You just made your first microapp!

You learned how pages work, how to modify those pages if you need to, you learned about changing and adding variables to buttons and components, how to order data in tables, add buttons that link somewhere else, and finally how to add subscribers to a microapp, schedule synchronizations and how to preview it.

There's a lot to take in, but hopefully running through this will make it easier on you when you come to do more heavy-weight microapps that might need things like authentication, and on-premises connections with the Connector Appliance.

Happy microapping :)
