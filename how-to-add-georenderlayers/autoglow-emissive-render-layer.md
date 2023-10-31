# AutoGlow/Emissive Render Layer

[`AutoGlowingGeoLayer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer/AutoGlowingGeoLayer.java) has built-in functionality for allowing your models to have emissive/glow textures. This is similar functionality to that of the vanilla Spider eyes, which glow in the dark.

## How to setup

Simply register the layer following [.](./ "mention") and then duplicate your model's texture file, renaming it from `<texturefilename>.png` to `<texturefilename>_glowmask.png` and then remove all the texture pixels using a Photo Editor such as [`Gimp`](https://www.gimp.org/downloads/) or [`Pixlr`](https://pixlr.com/e/) that you don't want to glow/be emissive.&#x20;
