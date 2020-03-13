---
versionFrom: 7.0.0
meta.Title: "Coding Standards"
meta.Description: "A guide to coding standards and naming conventions in Umbraco core."
---

# Coding standards and naming conventions

_Coding standards and naming conventions for all languages used in the Umbraco core_

## Naming conventions

### HTML & CSS
* CSS class names will be **.lowercase-hyphenated-names**
* HTML IDs will be **camelCasedNames**

### JavaScript
* Namespaces: **ProperCase/PascalCase**
* Class names: **camelCase**
* Method names: **camelCase**
* Property names: **camelCase**
* Private property names: **_camelCase**

### File names

* All file names throughout the solution will be **ProperCase/PascalCase** - this is extremely important for Visual Studio so that the generated class names follow the correct C&#35; naming conventions
* **However**, there is one exception to this rule, in v7 the AngularJs project (*Umbraco.Web.UI.Client*) all file names need to follow the convention for that project which is that all file names are **lowercased**

### C&#35;
When developing new Class libraries we will be adhering as closely as possible to the official guidelines as proposed by Microsoft [https://msdn.microsoft.com/en-us/library/ms229042.aspx](https://msdn.microsoft.com/en-us/library/ms229042.aspx)

Another good reference is [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries](https://www.oreilly.com/library/view/framework-design-guidelines/9780321545671)  book by Krzysztof Cwalina and Brad Abrams

Resharper settings are included with the solution, so developers can cleanup code in a consistent manner.
