# Basic Entity Setup

## Setting up the Entity class

Please see [CrawlerMonsterEntity](https://github.com/AzureDoom/Better-Crawling-ML/blob/1.20.1/Common/src/main/java/mod/azuredoom/bettercrawling/CrawlerMonsterEntity.java), which has everything you would need to call in your entity class, or you can simply use this base class for your entities!

## Setting up the Render if you are using AzureLib for your entity

In your render class, in the [preRender](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/GeoRenderer.java#L195) and [postRender](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/GeoRenderer.java#L202) methods, simply add the following:

```java
@Override
public void preRender(PoseStack poseStack, YourEntity animatable, BakedGeoModel model, MultiBufferSource bufferSource, VertexConsumer buffer, boolean isReRender, float partialTick, int packedLight, int packedOverlay, float red, float green, float blue, float alpha) {
	Constants.onPreRenderLiving(animatable, partialTick, poseStack);
	super.preRender(poseStack, animatable, model, bufferSource, buffer, isReRender, partialTick, packedLight, packedOverlay, red, green, blue, alpha);
}

@Override
public void postRender(PoseStack poseStack, YourEntity animatable, BakedGeoModel model, MultiBufferSource bufferSource, VertexConsumer buffer, boolean isReRender, float partialTick, int packedLight, int packedOverlay, float red, float green, float blue, float alpha) {
	super.postRender(poseStack, animatable, model, bufferSource, buffer, isReRender, partialTick, packedLight, packedOverlay, red, green, blue, alpha);
	Constants.onPostRenderLiving(animatable, partialTick, poseStack, bufferSource);
}
```

## Setting up the Render if you are using vanilla mob rendering

You don't have to do anything! This is handled for you via the [MixinLivingRenderer](https://github.com/AzureDoom/Better-Crawling-ML/blob/1.20.1/Common/src/main/java/mod/azuredoom/bettercrawling/mixin/client/MixinLivingRenderer.java) mixin!
