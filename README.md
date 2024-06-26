# Code Examples for What Editors Really Want

Here are some code examples to show you some practical ways for giving your editors and writers a better Wagtail experience.

## Set parent pages and subpages whenever possible

If there is only one child page type for a particular parent page, then you can reduce the number of clicks that writers and editors have to make by adding some simple settings. In this example blog project `BlogIndexPage` will only ever have `BlogPage` as a child. So we can add the following line to our `BlogPage` model:

```
parent_page_types = ['blog.BlogIndexPage']

```
And the following line can be added to the `BlogIndexPage` model:

```

subpage_types = ['blog.BlogPage']

```
With those settings in place, a new `BlogPage` will be created automatically whenever an editor adds a child page to their `BlogIndexPage`.

## Use page descriptions to make sure editors can keep track of page types

Descriptive names like "Blog Page" are very useful to editors. But if your Wagtail website has a lot of different page types or editors are managing multiple instances, it can be difficult to remember what the page is for. Help them out by using the Page descriptions feature in your Page models.

In this example, we're going to add descriptions to `HomePage` `BlogIndex Page` and `BlogTagIndexPage`. You can add a description to `BlogPage` as well to be consistent, but it's not really going to show up in a menu since we've already connected it to `BlogIndexPage` as a subpage.

For `HomePage`:

```
    page_description = "This page type is for the home page of the site. There should only be one home page."
```

For `BlogIndexPage`:

```
    page_description = "Use this page type for listing blog posts."

```

For `BlogTagIndexPage`:

```
    page_description = "This page type is for listing blog posts associated with a particular tag. There should only be one page with this type."

```

## Help editors make their art look good with images and help text

Most editors are responsible for resizing images to use in blog posts but they rarely know what pixel dimensions they need to make their images look good in a particular layout. They don't always have access to the frontend or template code either, so you can help them out but giving them prompts about the minimum image dimensions that are needed to make their images look stunning in a page layout.

We're going to go over two great ways to prompt editors about this information: help text and help panels. Let's add both of these examples to `BlogPageGalleryImage`.

First, `help_text`can be added to the `FieldPanel` for `image`

 ```
FieldPanel("image", help_text="Image size should be at least 1024 x 768 pixels."),
 ```

 Then, we can also add a `HelpPanel` above that `FieldPanel` to provide some additional information:

 ```
HelpPanel("Choose at least 1-3 images for each blog post."),

 ```

 The final version of the panels section for `BlogPageGalleryImage` should look like this:

 ```
 panels = [
        HelpPanel("Choose at least 1-3 images for each blog post."),
        FieldPanel("image", help_text="Image size should be at least 1024 x 768 pixels."),
        FieldPanel("caption"),
    ]
 ```

 You could use validation to check the dimensions of the images as well but that would probably be too heavy-handed for most editors. If they are in a rush and trying to publish, having a slightly fuzzy image is considered an acceptable sacrifice to meet a deadline.

## Use labels or duplicate text to let editors know what's going on in their templates

If you are adding failsafes or backups to your templates, then you should communicate it to your editors in some way. For example, we include this failsafe line for the search description on Wagtail.org:

```
content="{% if page.social_text %}{{ page.social_text }}{% elif page.listing_intro %}{{ page.listing_intro }}{% elif page.introduction %}{{ page.introduction|truncatechars:200 }}{% endif %}"
```

And in this sample project, there is a small backup line that you can finde here:

```
{% if page.seo_title %}{{ page.seo_title }}{% else %}{{ page.title }}{% endif %}
```

Editors aren't going to know right away that this code is there. Some will figure it out by trial and error, but why do that when we can let them know what's going on with a label?

We're going to adjust the promote panels code with a label so that our editors are aware that the title will be use by default.

```
promote_panels = (
        [HelpPanel("NOTE: If you do not fill out a title tag, the page title will be used by default.")]
        + Page.promote_panels
    )
```

## Custom Validation

We have another talk at Wagtail Space US on custom validation by Scott Cranfill that I will link to here. But I wanted to say that custom validation is another great way to help editors help themselves and remember to include key parts of their content.