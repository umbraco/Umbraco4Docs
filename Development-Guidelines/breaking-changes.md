---
versionFrom: 7.0.0
---

# Breaking changes

_Defines what a breaking change is in regards to the Umbraco core codebase and describes how to deal with required breaking changes._

## What is a breaking change?

This section describes what a breaking change is in regards to the Umbraco codebase. Generally breaking changes are only made on major releases, however, in minor releases there may be changes to the codebase that some developers may consider 'breaking' as well. The following points describe what changes to the core codebase are or are not considered breaking changes.

## Security

If a security issue arises with part of the Umbraco code base that requires changing the behavior or signatures of the code and no other option is available to mitigate the security issue, a breaking change will be released. This will be a minor version of Umbraco. If this scenario occurs, there will be documentation available for how to update your site to the fixed version.

## Dependencies

If a c# or JavaScript dependency is added or removed from the code base, a nuget package or the exported release zip file, it is not considered a breaking change. However, we will mark this as "breaking" in the release notes to indicate to the users that a dependency has changed. In the circumstance that a dependency is removed, you may have to manually add it to your own project if you depend on it.

## API Design

As the product evolves and more features are added, there may be rare circumstances where a current API's design falls short of the new feature set and doesn't provide a very friendly developer experience. If this circumstance occurs, it may be required that existing API signatures must evolve during a minor release. In this scenario investigation will be done to see what alternatives might exist to avoid sharp breaking changes though in some cases it will not be possible. Generally changes like this would be done in major versions but will be done in a minor version if the pros of changing these API signatures outweigh the cons of both breaking the signatures and releasing a major version.

## General codebase

### Non-breaking

* Any changes to the installation objects, classes, interfaces, views, client files, etc... are not considered breaking changes

* Any changes to the interfaces in the namespaces `Umbraco.Core.Services`, `Umbraco.Core.Models`, `Umbraco.Core.Models.Membership`,  are not considered breaking changes
    * Even though changes to an interface are normally considered breaking, these interfaces should not be implemented except by the core code and therefore we do not consider changes to these interfaces as breaking changes.

* Changes to class inheritance are not considered breaking if they do not break API usage
    * Take the following for example, if in v6 a class exists called `MyClass` with the following structure:

        ```csharp
        public class MyClass
        {
            public int Id {get;set;}
            public string Name {get;set;}
        }
        ```

    * Then in v6.1 the class structure changes to this:

        ```csharp
        public class MyClass : MySubClass
        {
            public string Name {get;set;}
        }

        public class MySubClass
        {
            public int Id {get;set;}
        }
        ```

    * With the above change, any API usage of MyClass will not break, however, if a developer is using reflection to target `MyClass` explicitly, in some cases this will break the reflection call. We **do not** consider these types of changes as breaking changes.
* Changes made to any non-public or non-protected property, methods, interfaces or classes are not considered breaking
    * Generally these types of changes will never break a developers usage unless they are using reflection to target non-public/non-protected objects.

### Breaking

* Changes made to public or protected method/property definitions
* Changes made to a public interface

## Backoffice Controllers

### Non-breaking

* Any changes made to MVC or WebApi controllers found in the namespace `Umbraco.Web.Editors` - these controllers are used solely for the purposes of Umbraco's own management of the backoffice data. These controllers are not meant for public consumption. It is recommended to create your own controllers for your own components.

## UI files

### Non-breaking

* Changes to CSS are not considered breaking changes
* Changes to HTML/Markup are not considered breaking changes
* Changes to images are not considered breaking changes
* Changes made to the inclusion, exclusion or location change of referenced JavaScript/CSS libraries
* Changes made to .less files are not considered breaking changes

### Breaking

* Changing the file location of CSS or Images is a breaking change

_It is advised to use Umbraco's Angular directives if you wish to create backoffice components, this will mean that you are not referencing CSS or Html markup directly_

## JavaScript

### Non-breaking

* Changes made to publicly accessible APIs that are not documented
* Changes made to publicly accessible API methods that are prefixed with an underscore (meaning these are flagged as internal)
* Changing the location of AngularJS service, controller, directive files

### Breaking

* Changes made to publicly accessible APIs that are documented
* Changing the location of JavaScript library files
* Changing the alias of AngularJS controller, service or directive

## Database

### Non-breaking

* Adding items to the schema (i.e. adding a column or a table) is not considered a breaking change
* Changing a column type if it does not cause data loss (i.e. changing from NVarchar to Text) is not considered breaking

### Breaking

* Removing an item from the schema (i.e. removing a column or a table) is considered a breaking change
* Changing a column type if it causes data loss or the result CLR type is different (i.e. Reducing an NVarchar length, or changing a column type from NVarchar to int)
