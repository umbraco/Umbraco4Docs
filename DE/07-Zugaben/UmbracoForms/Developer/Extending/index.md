---
versionFrom: 7.0.0
versionTo: 10.0.0
meta.Title: "Extending Umbraco Forms"
---

# Extending

## [Provider model](Adding-a-Type.md)

Most parts of Forms use a provider model, which makes it quicker to add new parts to the application.
The model uses the notion that everything must have a type to exist. The type defines the capabilities of the item. For instance a Textfield on a form has a FieldType, this particular field type enables it to render an input field and save text strings. The same goes for workflows, which have a workflow type, datasources which have a datasource type and so on. Using the model you can seamlessly add new types and thereby extend the application.
In the current version it is possible to add new Field types, Data Source Types, Prevalue Source Types, Export Types and Workflow Types.

## [Field types](Adding-a-Fieldtype.md)

A field type handles rendering of the UI for a field in a form. It renders a standard ASP.NET webcontrol and is able to return a list of values when the form is saved.

## Data Source Types

A data source type enables Umbraco Forms to connect to a custom source of data. A datasource can consist of any kind of storage as long as it possible to return a list of fields Umbraco Forms can map values to. For example: a Database data source can return a list of columns Forms can send data to, which enables Umbraco Forms to map a form to a data source. A data source type is responsible for connecting Forms to external storage.

## Prevalue Source Types

A prevalue source type can connect to a 3rd party storage and retrieve a collection of values which can be used on fields which support prevalues. The prevalue source is responsible for connecting to the source and retrieving the collection of values. A prevalue source type can also implement edit capabilities so new items can be added/updated/deleted directly from the form editor.

## [Workflow Types](Adding-a-Workflowtype.md)

A workflow can be executed each time a form changes state (when it is submitted for instance). A workflow is responsible for executing logic which can modify the record or notify 3rd party systems.

## [Export Types](Adding-a-Exporttype.md)

Export types are responsible for turning form records into any other data format, which is then returned as a file.

## [Validation](Adding-an-Event-Handler.md)

Form events are raised during the submission life cycle and can be handled for executing custom logic.

---

Prev: [Umbraco Forms in the Database](../Forms-in-the-Database/index.md) &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; &emsp; Next: [Configuration](../Configuration/index.md)
