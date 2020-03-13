---
versionFrom: 8.0.0
---

# Add a Blog Post Publication Date

In this lesson, you will learn how to add a new date property to blog posts.

## Outcome

Content editors can specify the publication date for a blog post. Blog posts are displayed with the most recent publication date appearing first.

## Takeaway

Learn how to:

* Add a new property to a Document Type
* Edit a Template to display the new property
* Sort a list of items by a new property

## Steps - Part One

1. In the **Settings** section, expand **Document Types**.
2. Click on *Blogpost*: this is the Document Type that defines the fields for this type of page.

    ![Blogpost Document Type](images/Blogpost-Document-Type.png)

3. The Document Type contains 2 groups: *Content* and *Navigation & SEO*.
    * At the bottom of the *Content* group select **Add property**.
    * This opens the **Property Editor** dialog window.

4. Give the property a name: *Publication Date*.
5. Give the property a description: *The date of the blog post. This is the date used to display the most recent posts first.*.

    * Always try to add a meaningful description to help your editors.

6. Select **Add editor** to specify what type of data is being stored.
    * We need a standard date, so select the **Use existing** tab, then click on the **Date Picker** icon.

7. In the validation section tick to say that the field is mandatory.
8. Submit to close the dialog and save the property.

    ![Property settings](images/property-settings.png)

9. Select the **Reorder** option near the top-right of the pane, then drag the new property to be after *Page Title*.
    * A logical order to your properties will make things easier for your editors.
10. *Save* the Document Type - a confirmation message should appear confirming that the Document Type was saved.

[Proceed to Part Two](part-2.md)
