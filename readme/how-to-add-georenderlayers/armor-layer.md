# Armor Layer

[`ItemArmorGeoLayer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer/ItemArmorGeoLayer.java) has built-in functionality for allowing your models to have emissive/glow textures. This is similar functionality to that of the vanilla Spider eyes, which glow in the dark.

## How to setup

Simply register the layer following [.](./ "mention") setup like so:

{% code overflow="wrap" %}
```java
/**
 * Custom renderer class for the {@link ExampleEntity}.
 * This class handles rendering of the entity, including an armor layer that renders equipped armor based on the entity's model bones.
 */
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {

    /**
     * Constructs a new {@link ExampleRenderer} with the specified rendering context.
     * Initializes the renderer with the {@link ExampleModel} and adds an armor layer for rendering equipped armor.
     *
     * @param context The {@link EntityRendererProvider.Context} used to manage rendering details like texture location and entity animations.
     */
    public ExampleRenderer(EntityRendererProvider.Context context) {
        super(context, new ExampleModel());

        // Adds a layer that renders armor equipped on the entity
        addRenderLayer(new ItemArmorGeoLayer<>(this) {

            /**
             * Retrieves the {@link ItemStack} for the armor item associated with a specific bone in the entity's model.
             * This method determines which armor item should be displayed based on the bone name.
             *
             * @param bone        The {@link GeoBone} representing the bone in the model.
             * @param animatable  The {@link DynamicExampleEntity} instance that is being animated and rendered.
             * @return The {@link ItemStack} for the specified armor bone, or {@code null} if no armor should be rendered.
             */
            @Nullable
            @Override
            protected ItemStack getArmorItemForBone(GeoBone bone, DynamicExampleEntity animatable) {
                // Return the appropriate armor item based on the bone name
                return switch (bone.getName()) {
                    case "left_boot_bone", "right_boot_bone" -> this.bootsStack;
                    case "left_armor_leg_bone", "right_armor_leg_bone" -> this.leggingsStack;
                    case "chestplate_bone", "right_sleeve_bone", "left_sleeve_bone" -> this.chestplateStack;
                    case "helmet_bone" -> this.helmetStack;
                    default -> null;
                };
            }

            /**
             * Determines the {@link EquipmentSlot} for the armor item based on the bone name and stack.
             * This method controls where the armor should be equipped on the entity.
             *
             * @param bone        The {@link GeoBone} representing the bone in the model.
             * @param stack       The {@link ItemStack} containing the armor item.
             * @param animatable  The {@link DynamicExampleEntity} instance being animated.
             * @return The {@link EquipmentSlot} corresponding to the bone.
             */
            @Nonnull
            @Override
            protected EquipmentSlot getEquipmentSlotForBone(GeoBone bone, ItemStack stack, DynamicExampleEntity animatable) {
                return switch (bone.getName()) {
                    case "left_boot_bone", RIGHT_BOOT -> EquipmentSlot.FEET;
                    case "left_armor_leg_bone", "right_armor_leg_bone" -> EquipmentSlot.LEGS;
                    case "right_sleeve_bone" -> !animatable.isLeftHanded() ? EquipmentSlot.MAINHAND : EquipmentSlot.OFFHAND;
                    case "left_sleeve_bone" -> animatable.isLeftHanded() ? EquipmentSlot.OFFHAND : EquipmentSlot.MAINHAND;
                    case "chestplate_bone" -> EquipmentSlot.CHEST;
                    case "helmet_bone" -> EquipmentSlot.HEAD;
                    default -> super.getEquipmentSlotForBone(bone, stack, animatable);
                };
            }

            /**
             * Retrieves the corresponding {@link ModelPart} for the specified bone and equipment slot.
             * This method is responsible for linking the bone to the correct part of the entity's model.
             *
             * @param bone        The {@link GeoBone} representing the bone in the model.
             * @param slot        The {@link EquipmentSlot} where the armor is equipped.
             * @param stack       The {@link ItemStack} containing the armor item.
             * @param animatable  The {@link DynamicExampleEntity} instance being animated and rendered.
             * @param baseModel   The {@link HumanoidModel} representing the base model of the entity.
             * @return The {@link ModelPart} that corresponds to the bone in the entity's model.
             */
            @Nonnull
            @Override
            protected ModelPart getModelPartForBone(GeoBone bone, EquipmentSlot slot, ItemStack stack, DynamicExampleEntity animatable, HumanoidModel<?> baseModel) {
                // Map the bone to the corresponding model part
                return switch (bone.getName()) {
                    case "left_boot_bone", "left_armor_leg_bone" -> baseModel.leftLeg;
                    case "right_boot_bone", "right_armor_leg_bone" -> baseModel.rightLeg;
                    case "right_sleeve_bone" -> baseModel.rightArm;
                    case "left_sleeve_bone" -> baseModel.leftArm;
                    case "chestplate_bone" -> baseModel.body;
                    case "helmet_bone" -> baseModel.head;
                    default -> super.getModelPartForBone(bone, slot, stack, animatable, baseModel);
                };
            }
        });
    }
}
```
{% endcode %}
