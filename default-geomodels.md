# Default GeoModels

[`Default GeoModels`](https://github.com/AzureDoom/AzureLib/tree/1.20/common/src/main/java/mod/azure/azurelib/model) are more advanced GeoModel setups that allow for easier setups.

## DefaultedEntityGeoModel <a href="#file-name-id-wide" id="file-name-id-wide"></a>

In your render that extends GeoEntityRenderer, instead of making a new GeoModel class, simply use:

`new` [`DefaultedGeoEntityModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/DefaultedEntityGeoModel.java)`(new ResourceLocation("yourmodid", "modelname"));`

This creates a new [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/GeoModel.java), and sets the following paths automagically, so simply save your assets as:

* Texture: `textures/entity/modelname.png`
* Model: `geo/entity/modelname.geo.json`
* Animation: `animations/entity/modelname.animation.json`

## DefaultedBlockGeoModel <a href="#file-name-id-wide" id="file-name-id-wide"></a>

In your render that extends [`GeoBlockRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/GeoBlockRenderer.java), instead of making a new GeoModel class, simply use:

`new` [`DefaultedGeoBlockModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/DefaultedBlockGeoModel.java)`(new ResourceLocation("yourmodid", "modelname"));`

This creates a new [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/GeoModel.java), and sets the following paths automagically, so simply save your assets as:

* Texture: `textures/block/modelname.png`
* Model: `geo/block/modelname.geo.json`
* Animation: `animations/block/modelname.animation.json`

## DefaultedItemGeoModel <a href="#file-name-id-wide" id="file-name-id-wide"></a>

In your render that extends [`GeoItemRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/GeoItemRenderer.java), instead of making a new GeoModel class, simply use:

`new` [`DefaultedGeoItemModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/DefaultedItemGeoModel.java)`(new ResourceLocation("yourmodid", "modelname"));`

This creates a new [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/GeoModel.java), and sets the following paths automagically, so simply save your assets as:

* Texture: `textures/item/modelname.png`
* Model: `geo/item/modelname.geo.json`
* Animation: `animations/item/modelname.animation.json`
