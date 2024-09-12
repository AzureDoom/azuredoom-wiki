# Block and Items Layer

[`BlockAndItemGeoLayer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer/ItemArmorGeoLayer.java) has built-in functionality for allowing your models to render with items or blocks in their hands.

## How to setup

Simply register the layer following [.](./ "mention") setup like so:

```java
/**
 * Custom renderer class for the {@link ExampleEntity}.
 * This class handles rendering of the entity, including the addition of item/block layers 
 * that allow the entity to hold or display items and blocks.
 */
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {

    /**
     * Constructs a new {@link ExampleRenderer} with the specified rendering context.
     * Initializes the renderer with the {@link ExampleModel} and adds a layer to render held items or blocks.
     *
     * @param context The {@link EntityRendererProvider.Context} used to manage rendering details like texture location and entity animations.
     */
    public ExampleRenderer(EntityRendererProvider.Context context) {
        super(context, new ExampleModel());

        // Adds a layer that renders items or blocks held by the entity
        addRenderLayer(new BlockAndItemGeoLayer<>(this) {

            /**
             * Retrieves the {@link ItemStack} associated with a specific bone in the entity's model.
             * This allows the entity to "hold" or display items based on the bone's name.
             *
             * @param bone        The {@link GeoBone} representing the bone in the model.
             * @param animatable  The {@link ExampleEntity} instance that is being animated and rendered.
             * @return The {@link ItemStack} for the specified bone, or {@code null} if no item should be rendered.
             */
            @Nullable
            @Override
            protected ItemStack getStackForBone(GeoBone bone, ExampleEntity animatable) {
                return switch (bone.getName()) {
                    case "handBoneInYourModel" -> animatable.getMainHandItem();
                    default -> null;
                };
            }

            /**
             * Determines the {@link ItemDisplayContext} (transformation type) for the given item stack based on the bone.
             * This controls how the item is positioned and rendered relative to the entity.
             *
             * @param bone        The {@link GeoBone} representing the bone in the model.
             * @param stack       The {@link ItemStack} that will be displayed.
             * @param animatable  The {@link ExampleEntity} instance that is being animated and rendered.
             * @return The {@link ItemDisplayContext} for transforming the item in the given context.
             */
            @Override
            protected ItemDisplayContext getTransformTypeForStack(GeoBone bone, ItemStack stack, ExampleEntity animatable) {
                return switch (bone.getName()) {
                    default -> ItemDisplayContext.THIRD_PERSON_RIGHT_HAND;
                };
            }

            /**
             * Renders the {@link ItemStack} for the specified bone, applying transformations to position it correctly.
             * The method is responsible for how the item/block appears visually when attached to the bone.
             *
             * @param poseStack      The {@link PoseStack} used to apply transformations to the item.
             * @param bone           The {@link GeoBone} representing the bone in the model.
             * @param stack          The {@link ItemStack} to be rendered.
             * @param animatable     The {@link ExampleEntity} instance being rendered.
             * @param bufferSource   The {@link MultiBufferSource} used for rendering the item/block.
             * @param partialTick    The partial tick time for smooth rendering.
             * @param packedLight    The light value for rendering the item/block.
             * @param packedOverlay  The overlay value for rendering.
             */
            @Override
            protected void renderStackForBone(PoseStack poseStack, GeoBone bone, ItemStack stack, ExampleEntity animatable, MultiBufferSource bufferSource, float partialTick, int packedLight, int packedOverlay) {
                // Rotate the item by 90 degrees on the X-axis
                poseStack.mulPose(Axis.XP.rotationDegrees(-90f));
                // Render the item with the provided parameters
                super.renderStackForBone(poseStack, bone, stack, animatable, bufferSource, partialTick, packedLight, packedOverlay);
            }
        });
    }
}
```
