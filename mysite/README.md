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