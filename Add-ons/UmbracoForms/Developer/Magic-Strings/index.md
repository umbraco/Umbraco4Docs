---
versionFrom: 7.0.0
needsV8Update: "true"
---

# Magic strings

Umbraco Forms has some magic strings that enable you to render values from various sources, such as session, cookies and Umbraco page fields.

## Where can I use magic strings?

Magic strings can be used in form fields as a label, description or default value. As an example they can be used in default values in hidden fields - normally in the form of referral codes from a session, cookie or request item.

These values can also be used for properties and settings in workflows. This means you can use name and email fields from a form to create a personal 'Thank you' email.

## Request

`[@SomeRequestItem]` this allows you to display an item from the current `HttpContext.Request` with the key of 'SomeRequestItem'.

Some examples of variables that are normally available in `HttpContext.Request`:

`[@Url]`: Insert the current URL
`[@Http_Referer]`: The previous visited URL (if available)
`[@Remote_Addr]`: The IP address of the visitor (stored by default by Umbraco)
`[@Http_User_Agent]`: The browser of the visitor

The variables are not case-sensitive.

## Session & Cookies
`[%SomeSessionOrCookieItem]` this allows you to display an item from the current `HttpContext.Session` with the key of 'SomeSessionOrCookieItem'. The session key can only contain alphanumeric chars and you cannot use dots for example. `[%Member.Firstname]` cannot be used, but `[%MemberFirstname]` can be used. You would have to fill these session keys yourself.

If the item cannot be found in the collection of session keys, it will then try to find the item from the `HttpContext.Cookies` collection with the same key.

## Umbraco Page field
`[#myUmbracoField]` this allows you to insert a property of that page and is based on the alias of the field. If your page has a property with the alias 'title', you can use `[#title]` in your form.

Some extra variables are:
`[#pageName]`: The nodename of the current page
`[#pageID]`: The node ID of the current page

## Recursive Umbraco Page field
`[$myRecursiveItem]` this allows you to parse the Umbraco document-type property myRecursiveItem. So if the current page does not contain a value for this then it will request it from the parent up until the root or until it finds a value.

## Parsing Umbraco Form field
`{myAliasForFormField}` this allows you to display the entered value for that specific field from the form submission. Used in workflows to send an automated email back to the customer based on the email address submitted in the form. The value here needs to be the alias of the field, and not the name of the field.

Some extra variables are:
`{record.id}`: The ID of the current record
`{record.updated}`: The updated date/time of the current record
`{record.created}`: The created date/time of the current record
`{record.umbracopageid}`: The Umbraco Page ID the form was submitted on
`{record.uniqueid}`: The unique ID of the current record
`{record.ip}`: The IP address that was used when the form was submitted
`{record.memberkey}`: The member key that was used when the form was submitted

:::warning
The current implementation doesn't use the field alias, but the caption. There's an open issue for this: https://github.com/umbraco/Umbraco.Forms.Issues/issues/120.
:::

## Parsing Member properties from a form submission
`{member.FOO}` with the prefix of member. in the same syntax above will allow you to retrieve information about the submission if it was submitted by a logged in member.

## How can I parse these values elsewhere in my C# code or Razor Views?
The `Umbraco.Forms.Data.StringHelpers` class contains helper methods for parsing magic strings:

```csharp
// Does not parse Record-related magic strings - they are removed.
public static string ParsePlaceHolders(HttpContext context, string value)

// Uses the passed in Record to parse Record-related magic strings
public static string ParsePlaceHolders(Record record, string value)

public static string ParsePlaceHolders(HttpContext context, Record record, string value)
```

There is also a public extension method `ParsePlaceHolders()` extending the `string` object in the `Umbraco.Forms.Core.Extensions` namespace. This can be used to replace the above tokens in a string, but it doesn't currently work with records/From field items.
