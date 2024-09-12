---
description: Thanks to MrSterner for this tip
---

# Render GeoArmor Item in GUI instead of 2D texture

In your Armor item class, do the createRenderer like for Fabric/NeoForge:

```java
/**
 * Creates a renderer and accepts a consumer to handle custom rendering logic for an armor item or object.
 * 
 * <p><strong>Note:</strong> In Minecraft 1.21+ the {@link Object} type is replaced with {@link RenderProvider}.</p>
 *
 * @param consumer A {@link Consumer} that accepts a rendering object, responsible for providing the custom renderers.
 */
@Override
public void createRenderer(Consumer<Object> consumer) {
    consumer.accept(new RenderProvider() {

        private GeoArmorRenderer<?> renderer;
        private GeoItemRenderer<?> itemRenderer;

        /**
         * Returns a custom item renderer for rendering models in the player's hand or inventory.
         * 
         * @return A {@link BuiltinModelItemRenderer} for the custom item model.
         */
        @Override
        public BuiltinModelItemRenderer getCustomRenderer() {
            // Lazily initialize the item renderer if it hasn't been created yet.
            if (this.itemRenderer == null) {
                this.itemRenderer = new ExampleItemRenderer();
            }
            return this.itemRenderer;
        }

        /**
         * Retrieves the humanoid armor model for the given entity and slot.
         * This method sets up a custom armor renderer for specific armor pieces on a humanoid entity.
         *
         * @param livingEntity    The {@link LivingEntity} to which the armor is being applied.
         * @param itemStack       The {@link ItemStack} containing the armor item.
         * @param equipmentSlot   The {@link EquipmentSlot} specifying which slot the armor is equipped in (e.g., head, chest, etc.).
         * @param original        The original {@link HumanoidModel} to be replaced or modified.
         * @return A {@link HumanoidModel} that represents the custom armor model for the specified slot.
         */
        @Override
        public HumanoidModel<LivingEntity> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<LivingEntity> original) {
            // Lazily initialize the armor renderer if it hasn't been created yet.
            if (this.renderer == null) {
                this.renderer = new ExampleArmorRenderer();
            }
            // Prepare the custom renderer for rendering the armor on the entity.
            this.renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
            return this.renderer;
        }
    });
}
```

and Forge 1.19.4 and older like so:

```java
/**
 * Initializes the client-side extensions for the armor item.
 * This method sets up the custom rendering logic for an armor item.
 *
 * @param consumer A {@link Consumer} that accepts an {@link IClientItemExtensions} object to handle client-side rendering extensions.
 */
@Override
public void initializeClient(Consumer<IClientItemExtensions> consumer) {
    consumer.accept(new IClientItemExtensions() {

        private GeoArmorRenderer<?> renderer;
        private GeoItemRenderer<?> itemRenderer;

        /**
         * Provides a custom item renderer for rendering the armor model in the player's hand or inventory.
         *
         * @return A {@link BuiltinModelItemRenderer} for the custom item model.
         */
        @Override
        public BuiltinModelItemRenderer getCustomRenderer() {
            // Lazily initialize the item renderer if it hasn't been created yet.
            if (this.itemRenderer == null) {
                this.itemRenderer = new ExampleItemRenderer();
            }
            return this.itemRenderer;
        }

        /**
         * Retrieves the humanoid armor model for the given entity and slot.
         * This method prepares and returns a custom armor renderer for a specific armor piece on a humanoid entity.
         *
         * @param livingEntity    The {@link LivingEntity} that the armor is being applied to.
         * @param itemStack       The {@link ItemStack} containing the armor item.
         * @param equipmentSlot   The {@link EquipmentSlot} indicating where the armor is equipped (e.g., head, chest, legs, feet).
         * @param original        The original {@link HumanoidModel} to be replaced or modified by the custom model.
         * @return A {@link HumanoidModel} representing the custom armor model for the specified slot.
         */
        @Override
        @NotNull
        public HumanoidModel<?> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<?> original) {
            // Lazily initialize the armor renderer if it hasn't been created yet.
            if (this.renderer == null) {
                this.renderer = new ExampleArmorRenderer();
            }
            // Prepare the custom renderer for rendering the armor on the entity.
            this.renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
            return this.renderer;
        }
    });
}

```
