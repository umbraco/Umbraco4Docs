---
versionFrom: 8.0.0
meta.Title: "Umbraco Heartcore Content Management"
meta.Description: "Documentation for Heartcore Content Management APIs"
---

# Content Management API

This is the management API for creating, updating and deleting content, media, languages, relations, members and member groups. It also allows you to retrieve content drafts as well as Content Types, Media Types and Member Types.

Common for the Content Management API is that you must be authenticated and authorized when performing any action against the endpoints listed below. This means that you must supply a Bearer Token via an Authorization header or an API Key via an Authorization or Api-Key header.

## [Content endpoints](content/)

Content endpoints for retrieving, creating, updating and deleting.

## [Content Type endpoints](content/type/)

Content Type endpoints for retrieving all available and specific content types. We also expose endpoints for publishing and unpublishing content.

## [Media endpoints](media/)

Media endpoints for retrieving, creating, updating and deleting.

## [Media Type endpoints](media/type/)

Media Type endpoints for retrieving all available and specific media types.

## [Language endpoints](language/)

Language endpoints for retrieving, creating, updating and deleting.

## [Member endpoints](member/)

Member endpoints for retrieving, creating, updating and deleting. We also expose endpoints for adding a member to member group and removing a member group from a member.

## [Member Group endpoints](member/group/)

Member Group endpoints for retrieving, creating and deleting.

## [Member Type endpoints](member/type/)

Member Type endpoints for retrieving all available and specific member types.

## [Relation endpoints](relation/)

Relation endpoints for retrieving, creating and deleting.

## [Relation Type endpoints](relation/type/)

Relation Type endpoints for retrieving specific relation types.

## [Umbraco Forms endpoints](forms/)

Umbraco Forms endpoints for retrieving and submitting forms.
