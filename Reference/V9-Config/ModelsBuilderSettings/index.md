---
versionFrom: 9.0.0
meta.Title: "Umbraco Models Builder Settings"
meta.Description: "Information on the models builder settings section"
state: complete
verified-against: beta-3
update-links: true
---

# Models builder settings

This section allows you to configure the Umbraco models builder, a complete section with default values can be seen here: 

```json
"Umbraco": {
  "CMS": {
    "ModelsBuilder": {
      "ModelsMode": "InMemoryAuto",
      "ModelsNamespace": "Umbraco.Cms.Web.Common.PublishedModels",
      "FlagOutOfDateModels": false,
      "ModelsDirectory": "~/umbraco/models",
      "AcceptUnsafeModelsDirectory": false,
      "DebugLevel": 0
    }
  }
}
```

Let's go through them one by one 

## Models mode

Specifies how the models builder will generate models and when to generate them, options are:

* `Nothing` - The modelsbuilder will not generate any models, this means that all views will use IPublishedContent, instead of strongly typed models.
* `InMemoryAuto` - Models will automatically be generated each time a content type change occurs, and will then be compiled, and loaded into memory dynamically. This means that the models are only availabe in views, however they will be available instantly.
* `SourceCodeManual` - Models will be generated as `.cs` files whenver a users clicks the "Generate models" button on the models builder dashboard, the models however will not be compiled and loaded into memory dynamically. This means that models are available everywhere, however the project needs to be recompiled and restarted for the new models, or model changes, to take effect.
* `SourceCodeAuto` - This mode behaves the same as `SourceCodeManual` with one difference, the generation of models happens automatically every time a content type change occurs.

## Models namespace

This setting allows you to customize the namespace of the generated models, for instance you might want to change this to something that aligns better with your project structure, such as `MySite.ContentModels`.

## Flag out of date models

This setting allows you to specify if a model should be flagged as out of date if its content type, or a datatype the content type depends on, are changed. When a model is flagged as out of date you will be able to see that you need to regenerated models in modelsbuilder dashboard. 

This setting is only really relevant if you use the `SourceCodeManual` models mode, since otherwise the models will be automatically regenerated, and will therefore never be out of date. 

If you set this setting to true while using an `Auto` mode, it will automatically be intepreted as false.

## Models directory 

Allows you to specify a custom directory for your generated models. By default this settings has to be a virtual directory, that is, it must start with `~/`, if needed `AccceptUnsafeModelsDirectory` can be set to true, to allow the path to be outside the website root, be aware though that this is a potential security risk.

## Accept unsafe models directory

By setting this to true, you specify that the models directory is allowed to be outside the websites root. This is not allowed by default since it can be a potential security risk.

## Debug level

This settings specifies the logging level for the models builder, by default this is set to 0, which means minimal logging, anything higher that 0 means increased logging. Be aware that this setting should only be set to something higher than 0 for development use, not on live sites.