# Block and Items Layer

[`BlockAndItemGeoLayer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer/ItemArmorGeoLayer.java) has built-in functionality for allowing your models to render with items or blocks in their hands.

## How to setup

Simply register the layer following [.](./ "mention") setup like so:

```java
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {
	public ExampleRenderer(EntityRendererProvider.Context context) {
		super(context, new ExampleModel());
		// Add the item/block layer
		addRenderLayer(new BlockAndItemGeoLayer<>(this) {
			@Nullable
			@Override
			protected ItemStack getStackForBone(GeoBone bone, ExampleEntity animatable) {
				return switch (bone.getName()) {
				case "handBoneInYourModel" -> animatable.getMainHandItem();
				default -> null;
				};
			}

			@Override
			protected ItemDisplayContext getTransformTypeForStack(GeoBone bone, ItemStack stack, ExampleEntity animatable) {
				return switch (bone.getName()) {
				default -> ItemDisplayContext.THIRD_PERSON_RIGHT_HAND;
				};
			}

			@Override
			protected void renderStackForBone(PoseStack poseStack, GeoBone bone, ItemStack stack, ExampleEntity animatable, MultiBufferSource bufferSource, float partialTick, int packedLight, int packedOverlay) {
				poseStack.mulPose(Axis.XP.rotationDegrees(-90f));
				super.renderStackForBone(poseStack, bone, stack, animatable, bufferSource, partialTick, packedLight, packedOverlay);
			}
		});
	}
}
```
