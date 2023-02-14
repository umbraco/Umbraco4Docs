---
versionFrom: 9.0.0
---

# umbProperty

The [umb-property](https://apidocs.umbraco.com/v10/ui#/api/umbraco.directives.directive:umbProperty) directive can along with [umb-property-editor](https://apidocs.umbraco.com/v10/ui#/api/umbraco.directives.directive:umbPropertyEditor) be used for rendering property editors in the backoffice.

The two directives are typically used together. For instance, if your Angular model has an array of properties, your view could look something like:

```html
<umb-property property="property" ng-repeat="property in properties">
    <umb-property-editor model="property"></umb-property-editor>
</umb-property>
```

As `properties` contains the model for each property, we can use `ng-repeat` to iterate over each property, which is then passed on to the two directives via the `property` and `model` attributes respectively.

For a basic property with a textbox, the model for the property can be defined as:

```javascript
var property = {
    alias: "myProperty",
    label: "My property",
    description: "This is my property.",
    value: "Cupcake ipsum dolor sit amet oat cake marzipan...",
    view: "textbox"
};
```

The `view` property specifies the URL to the property editor that should be used for this property. To use one of the built-in property editors in Umbraco, you can specify the alias (eg. `textbox`) rather than the full URL to the view (eg. `/umbraco/Views/propertyeditors/textbox/textbox.html`).

You can see a list of all the built-in property editors in the [propertyeditors folder on GitHub](https://github.com/umbraco/Umbraco-CMS/tree/v11/contrib/src/Umbraco.Web.UI.Client/src/views/propertyeditors).
