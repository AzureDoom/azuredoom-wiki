# Armor Layer

[`ItemArmorGeoLayer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer/ItemArmorGeoLayer.java) has built-in functionality for allowing your models to have emissive/glow textures. This is similar functionality to that of the vanilla Spider eyes, which glow in the dark.

## How to setup

Simply register the layer following [.](./ "mention") setup like so:

```java
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {
	public ExampleRenderer(EntityRendererProvider.Context context) {
		super(context, new ExampleModel());
		// Add the armor layer
		addRenderLayer(new ItemArmorGeoLayer<>(this) {
			@Nullable
			@Override
			protected ItemStack getArmorItemForBone(GeoBone bone, DynamicExampleEntity animatable) {
				// Return the items relevant to the bones being rendered for additional rendering
				return switch (bone.getName()) {
					case "left_boot_bone", "right_boot_bone" -> this.bootsStack;
					case "left_armor_leg_bone", "right_armor_leg_bone" -> this.leggingsStack;
					case "chestplate_bone", "right_sleeve_bone", "left_sleeve_bone" -> this.chestplateStack;
					case "helmet_bone" -> this.helmetStack;
					default -> null;
				};
			}
			
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

			@Nonnull
			@Override
			protected ModelPart getModelPartForBone(GeoBone bone, EquipmentSlot slot, ItemStack stack, DynamicExampleEntity animatable, HumanoidModel<?> baseModel) {
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
