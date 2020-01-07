---
layout: doc
description: How to build a simple microapp using a public RSS feed
---
# Title
Building a simple Citrix microapp that shows blog posts from a Wordpress RSS feed
## Contributors

**Author:** [Phil Wiffen](https://twitter.com/phil_wiffen)


---

<!-- wp:image {"id":1715,"align":"center","width":461,"height":304} -->
<div class="wp-block-image"><figure class="aligncenter is-resized">![](2019-12-17-23_59_41-Window.png)</figure></div>
<!-- /wp:image -->

## Scope

This post will cover how to setup a simple microapp that anyone with access to the Citrix microapps service can build, using public URLs, with no authentication requirements. We'll be using the RSS integration to talk to a Wordpress RSS feed and build microapps from that.

## Why bother?

<!-- /wp:heading -->

<!-- wp:paragraph -->

I've spent the last few months learning about microapps, and implementing them into Citrix Engineering's pre-release Workspace environments - and honestly, I wish I had a guide like this to follow when I first started. So here we are :)

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

My hope is that following this will help you get familiar with the concepts, before you dive into the heavier stuff, like accessing internal APIs, or figuring out the details around Oauth 2.0 authentication.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

By following this post you'll learn:

<!-- /wp:paragraph --> <!-- wp:list -->

*   How to add an Integration and a microapp
*   How to make changes to how a microapp displays data

<!-- wp:heading -->

## What are we building?

<!-- /wp:heading -->

<!-- wp:paragraph -->

We'll build a Citrix Blog posts microapp, which uses the [RSS Out-of-the-box integration](https://docs.citrix.com/en-us/citrix-microapps/set-up-template-integrations/integrate-rss.html) to connect to the Citrix Blogs RSS feed, which happens to be in a Wordpress format and:

<!-- /wp:paragraph -->

<!-- wp:list -->

*   Notifies Workspace when there's a new blog post
*   Enables colleagues to view blog posts from Workspace
*   Allows colleagues to view a list of blog posts in a searchable table
<!-- /wp:list -->

<!-- wp:heading -->

## Before you start

<!-- /wp:heading -->

<!-- wp:paragraph -->

This guide assumes that you already have access to the Microapps service in your Citrix Cloud account.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Familiarise yourself with the following microapps concepts by [reading the documentation](https://docs.citrix.com/en-us/citrix-microapps.html#terminology?)

<!-- /wp:paragraph -->

<!-- wp:list -->

*   Integration
*   Microapp
*   Notification
*   Page
*   Action
<!-- /wp:list -->

<!-- wp:paragraph -->

I'll be using the above terms liberally, so knowing what they mean will help.

<!-- /wp:paragraph -->

<!-- wp:heading -->

## Let's build it!

<!-- /wp:heading -->

<!-- wp:heading {"level":3} -->

### We're going to:

<!-- /wp:heading -->

<!-- wp:list {"ordered":true} -->

1.  Add an RSS Integration to sync Citrix Blog posts
2.  Change the name of the microapp so it's more intuitively named in Workspace Actions
3.  Change the sort order of Blog posts to: Date, Descending
4.  Change the Description field of the Item Detail page to show HTML content for prettier viewing
5.  Set the Title of the blog post to be in the header of the page
6.  Add a "View Post" button, that goes to the Blog Post online
7.  Setup the Synchronization Schedule
8.  Add Subscribers to the microapp (so they can see Notifications and Actions)
<!-- /wp:list -->

<!-- wp:heading {"level":3} -->

### Add the Integration

<!-- /wp:heading -->

<!-- wp:paragraph -->

Go to the Microapps Admin page, and choose "Add New Integration"

<!-- /wp:paragraph -->

<!-- wp:image {"id":1589} -->
<figure class="wp-block-image">![](2019-12-16-22_00_30-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Choose the RSS Integration

<!-- /wp:paragraph -->

<!-- wp:image {"id":1598} -->
<figure class="wp-block-image">![](2019-12-16-22_12_50-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Call the Integration a name, such as  Citrix Blogs (RSS), and enter the URL to the Wordpress RSS feed. In this example, for Citrix Blogs, we're using http://feeds.feedblitz.com/citrix&x=1 

<!-- /wp:paragraph -->

<!-- wp:image {"id":1591} -->
<figure class="wp-block-image">![](2019-12-16-22_04_39-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

You'll then be taken to the Microapps list of integrations, and you'll see the new integration. Inside, it'll already have a microapp configured called "Items", and you'll see it has Synchronized

<!-- /wp:paragraph -->

<!-- wp:image {"id":1596} -->
<figure class="wp-block-image">![](2019-12-16-22_08_53-Microapps-Admin-1024x189.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Because the RSS Integration is deliberately generic - we need to make a number of tweaks to make the Blog microapp nicer to use and look at.

<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->

### Change the name of the Microapp

<!-- /wp:heading -->

<!-- wp:paragraph -->

We change the name of the microapp, because this is how it appears in the Actions pane in Workspace. If we keep it as "Items", that's not particularly user friendly. In our environments, we rename this microapp to Citrix Blogs, so people will know what they get when they click the Action. 

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

To change the Microapp name:

<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->

1.  Click on "Items"
2.  Go to Properties
3.  Change the App Name, and the App Description
4.
5.  Click Save
<!-- /wp:list -->

<!-- wp:image {"id":1602,"width":440,"height":542} -->
<figure class="wp-block-image is-resized">![](2019-12-16-22_18_04-Microapps-Admin-2.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Here's a comparison of how they'd look in Workspace. I much prefer it to say Citrix Blogs. rather than Items :)

<!-- /wp:paragraph -->

<!-- wp:image {"id":1604} -->
<figure class="wp-block-image">![](2019-12-16-22_35_40-Citrix-Workspace.png)</figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->

### Change the sort order of Blog posts to Date, Descending 

<!-- /wp:heading -->

<!-- wp:paragraph -->

Out of the box, the Items table is not sorted by Date. Let's fix that so it's suitable for a Blog post list that shows the latest posts at the top of the table.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

In the Citrix Blogs microapp, click on Pages, then click on Items

<!-- /wp:paragraph -->

<!-- wp:image {"id":1611} -->
<figure class="wp-block-image">![](2019-12-16-22_47_27-Microapps-Admin-3-1024x312.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Click the Table, so that it's highlighted (a blue x will appear in the top-right of the table). Then click Set Order

<!-- /wp:paragraph -->

<!-- wp:image {"id":1627} -->
<figure class="wp-block-image">![](2019-12-16-23_16_53-Microapps-Admin-1-1024x442.png)</figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true} -->

1.  Set the Order by to: items, published_at, Descending.
2.  Click Save
3.
<!-- /wp:list -->

<!-- wp:image {"id":1613} -->
<figure class="wp-block-image">![](2019-12-16-22_51_30-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Bonus points: Change the table Label from Items to Blog Posts. Again, this just makes it look nicer.

<!-- /wp:paragraph -->

<!-- wp:image {"id":1614} -->
<figure class="wp-block-image">![](2019-12-16-23_02_47-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->

### Displaying HTML content from the blog posts

<!-- /wp:heading -->

<!-- wp:paragraph -->

Just FYI: This HTML component is in the product at GA because I had issues with showing blog content in the microapp page, and the Product Management team did amazing work to accelerate their plans for this functionality. It sounds small, but it's just one tangible example of what I (and my team) do inside Citrix - we use the product, we feedback, we help make things better

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Out of the box, the RSS Integration shows raw text. It works really well for many kinds of feeds/data, but for some RSS feeds, the fields have HTML embedded in them.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Out of the box, it looks like this:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1636} -->
<figure class="wp-block-image">![](2019-12-17-21_24_46-Microapps-Admin-5-588x1024.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

And we're going to tidy it up so it looks like this:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1701} -->
<figure class="wp-block-image">![](2019-12-17-22_36_03-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->

### Add the blog title to the header of the Page

<!-- /wp:heading -->

<!-- wp:paragraph -->

Set the Blog Title as the page title and remove the existing title. This makes the title look much nicer:

<!-- /wp:paragraph -->

<!-- wp:list -->

*   Go to Pages, then click on Item Detail
*   Click the Back button in the Builder viewer
*   In the right-hand pane, set the Title Template from blank, to {{title}}
<!-- /wp:list -->

<!-- wp:image {"id":1703} -->
<figure class="wp-block-image">![](2019-12-17-21_25_46-1024x265.png)</figure>
<!-- /wp:image -->

<!-- wp:list -->

*   Click the Title text in the main body of the Builder viewer, and delete it (because the title is now shown at the top of the page)
*   This will now show the Blog post title in the Page header (and looks wayyyyyyy nicer)
<!-- /wp:list -->

<!-- wp:paragraph -->

Replace the Description default Test component, with the HTML Content component:

<!-- /wp:paragraph -->

<!-- wp:list {"ordered":true} -->

1.  Drag the HTML Content component so it goes above where the current Description text component is
2.
<!-- /wp:list -->

<!-- wp:image {"id":1649} -->
<figure class="wp-block-image">![](2019-12-17-21_33_41-Microapps-Admin-10-1024x630.png)</figure>
<!-- /wp:image -->

<!-- wp:list {"ordered":true} -->

1.  Set the Label (you can make it blank - it's not required) and set the Data Table to "Items" and the Data column to "Description"
2.  Delete the other Description component, by clicking on it, then clicking the X in the top=right hand corner.P
<!-- /wp:list -->

<!-- wp:image {"id":1678} -->
<figure class="wp-block-image">![](2019-12-17-21_47_49-1024x375.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

It'll then populate the HTML component and look much nicer!

<!-- /wp:paragraph -->

<!-- wp:image {"id":1698} -->
<figure class="wp-block-image">![](2019-12-17-22_01_28-3-1024x580.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Personally, I remove the Categories Table, as it serves no purpose in this view

<!-- /wp:paragraph -->

<!-- wp:image {"id":1697} -->
<figure class="wp-block-image">![](2019-12-17-22_06_53-Microapps-Admin-1.png)</figure>
<!-- /wp:image -->

<!-- wp:heading -->

## Add a button to link to the Blog Post

<!-- /wp:heading -->

<!-- wp:paragraph -->

Finally, let's add a button to link to the Blog Post. This is a nice way to allow people to open the blog post if they want to read more than just the lede.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Drag the "Button" button to the bottom of the page:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1696} -->
<figure class="wp-block-image">![](2019-12-17-22_08_52-Microapps-Admin-1-1024x925.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Then, set the button up like so...

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Change the Button Label from Button, to "View Post" (or whatever you'd like it to say :))

<!-- /wp:paragraph -->

<!-- wp:image {"id":1699} -->
<figure class="wp-block-image">![](2019-12-17-22_34_52-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Now we setup the link part.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Click Actions, on the right hand side:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1688} -->
<figure class="wp-block-image">![](2019-12-17-22_09_54-Microapps-Admin-1-1024x207.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Then on the Drop down, choose "Go to URL"

<!-- /wp:paragraph -->

<!-- wp:image {"id":1685} -->
<figure class="wp-block-image">![](2019-12-17-22_10_38-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Expand the Goto URL, then click on Insert Variable

<!-- /wp:paragraph -->

<!-- wp:image {"id":1684} -->
<figure class="wp-block-image">![](2019-12-17-22_11_27-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

From the dropdown, choose "url" and click Insert:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1695} -->
<figure class="wp-block-image">![](2019-12-17-22_15_22-1.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

You'll then see the field populated with {{url}}. This will insert the Blog Post's URL into the button, and will launch the site when clicked.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Preview the microapp to see your handiwork:

<!-- /wp:paragraph -->

<!-- wp:image {"id":1704} -->
<figure class="wp-block-image">![](2019-12-17-22_44_05-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

Looks good!

<!-- /wp:paragraph -->

<!-- wp:image {"id":1710} -->
<figure class="wp-block-image">![](2019-12-17-23_06_33-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:image {"id":1705} -->
<figure class="wp-block-image">![](2019-12-17-22_44_43-Microapps-Admin-1024x732.png)</figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->

### Set a Synchronization Schedule

<!-- /wp:heading -->

<!-- wp:paragraph -->

Now we've made the microapp, with its pages, we should set a Synctonization Schedule.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

The schedule is up to you and the feed you're pulling in. For Citrix Blogs, we set this to once every hour, which is a good balance between keeping things up to date, but not hitting the website too much with requests.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

After a sync happens, if a new blog post entry is found a notification is sent to Subscribers to that microapp, and it appears in the Workspace feed (and, if enabled, a Push Notification to the person's device with Citrix Workspace app, too)

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

_Please note: The first time you sync - no Notifications will be generated. So when you look in your feed, there won't be any Notifications yet. This is because notifications are generated the **next** time a sync occurs **and **there's new blog posts. The first sync will simply load up the microapps cache. You'll need to wait for a new blog post to be posted into the RSS, and for a sync to happen, for a notification to be generated._

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

You set the Synchronization by going to the Microapp Integrations page, clicking the three dots next to the integration and choosing Synchronization

<!-- /wp:paragraph -->

<!-- wp:image {"id":1706} -->
<figure class="wp-block-image">![](2019-12-17-22_47_32-Microapps-Admin-1024x165.png)</figure>
<!-- /wp:image -->

<!-- wp:heading {"level":3} -->

### Adding Subscribers to the microapp

<!-- /wp:heading -->

<!-- wp:paragraph -->

The final step of this process: Giving people access to the microapp itself. Without being granted this, people won't see the microapp, or the notifications

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

To give people access, Click the three dots next to the microapp, and choose Subscriptions

<!-- /wp:paragraph -->

<!-- wp:image {"id":1707} -->
<figure class="wp-block-image">![](2019-12-17-22_51_15-Microapps-Admin-1024x232.png)</figure>
<!-- /wp:image -->

<!-- wp:paragraph -->

In the example below, I'm showing that you can add Security Groups, as well as individual users. This can help you test, and gradually roll out the microapp in a live environment in phases, to help ensure quality and user acceptance.

<!-- /wp:paragraph -->

<!-- wp:image {"id":1708} -->
<figure class="wp-block-image">![](2019-12-17-22_52_51-Microapps-Admin.png)</figure>
<!-- /wp:image -->

<!-- wp:heading -->

## Pat yourself on the back

<!-- /wp:heading -->

<!-- wp:paragraph -->

You just made your first microapp!

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

You learned how pages work, how to modify those pages if you need to, you learned about changing and adding variables to buttons and components, how to order data in tables, add buttons that link somewhere else, and finally how to add subscribers to a microapp, schedule synchronizations and how to preview it.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

There's a lot to take in, but hopefully running through this will make it easier on you when you come to do more heavy-weight microapps that might need things like authentication, and on-premises connections with the Connector Appliance.

<!-- /wp:paragraph -->

<!-- wp:paragraph -->

Happy microapping :)

<!-- /wp:paragraph -->
