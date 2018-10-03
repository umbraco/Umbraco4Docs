# DataTypeService

**Applies to Umbraco 6.x and newer**

The DataTypeService acts as a "gateway" to Umbraco data for operations which are related to DataTypes and DataTypeDefinitions.

[Browse the API documentation for DataTypeService](https://our.umbraco.com/apidocs/csharp/api/Umbraco.Core.Services.DataTypeService.html).

 * **Namespace:** `Umbraco.Core.Services` 
 * **Assembly:** `Umbraco.Core.dll`

All samples in this document will require references to the following dll:

* Umbraco.Core.dll

All samples in this document will require the following using statements:
	
	using Umbraco.Core;
	using Umbraco.Core.Models;
	using Umbraco.Core.Services;

**Please note that this page will be updated with samples and additional information about the methods listed below**

## Getting the service
The DataTypeService is available through the `ApplicationContext`, but the if you are using a `SurfaceController` or the `UmbracoUserControl` then the DataTypeService is available through a local `Services` property.

	Services.DataTypeService

Getting the service through the `ApplicationContext`:

	ApplicationContext.Current.Services.DataTypeService

## Methods

### .GetDataTypeDefinitionById(int id)
Gets a `DataTypeDefinition` by its `Int` Id.

### .GetDataTypeDefinitionById(Guid id)
Gets a `DataTypeDefinition` by its unique `Guid` Id.

### .GetAllDataTypeDefinitions(params int[] ids)
Gets all `DataTypeDefinition` objects or those with the ids passed in.

### .Save(IDataTypeDefinition dataTypeDefinition, int userId = 0)
Saves a `DataTypeDefinition` object.

### .Delete(IDataTypeDefinition dataTypeDefinition, int userId = 0)
Deletes a `DataTypeDefinition` object.

### .GetDataTypeById(Guid id)
Gets an `IDataType`  by its unique Id.

### .GetAllDataTypes()
Gets a complete list of all registered DataTypes as `IDataType` objects.

### .GetDataTypeDefinitionByControlId(Guid id)
Gets a `DataTypeDefinition` by its control Id.

### .GetPreValueAsString(int id)
Gets a `String` containing all prevalues for a `DataTypeDefinition`.

### .GetPreValuesByDataTypeId(int id)
Gets an `IEnumerable<String>` containing all prevalues for a `DataTypeDefinition`.

### .GetPreValuesCollectionByDataTypeId(int id)
Gets a `PreValueCollection` for a `DataTypeDefinition`.
