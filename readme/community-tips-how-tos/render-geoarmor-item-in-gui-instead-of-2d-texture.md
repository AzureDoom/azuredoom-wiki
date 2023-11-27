---
description: Thanks to MrSterner for this tip
---

# Render GeoArmor Item in GUI instead of 2D texture

In your Armor item class, do the createRenderer like for Fabric:

```java
@Override
public void createRenderer(Consumer<Object> consumer) {
    consumer.accept(new RenderProvider() {
        private GeoArmorRenderer<?> renderer;
        private GeoItemRenderer<?> itemRenderer;

        @Override
        public BuiltinModelItemRenderer getCustomRenderer() {
            if(this.itemRenderer == null) {
                this.itemRenderer = new ExampleItemRenderer();
            }
            return this.itemRenderer;
        }

        @Override
        public HumanoidModel<LivingEntity> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<LivingEntity> original) {
            if(this.renderer == null) {
                this.renderer = new ExampleArmorRenderer();
            }
            this.renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
            return this.renderer;
            }
    });
}
```

and Forge like so:

```java
@Override
public void initializeClient(Consumer<IClientItemExtensions> consumer) {
    consumer.accept(new IClientItemExtensions() {
        private GeoArmorRenderer<?> renderer;
        private GeoItemRenderer<?> itemRenderer;

        @Override
        public BuiltinModelItemRenderer getCustomRenderer() {
            if(this.itemRenderer == null) {
                this.itemRenderer = new ExampleItemRenderer();
            }
            return this.itemRenderer;
        }

        @Override
        @NotNull HumanoidModel<?> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<?> original) {
            if(this.renderer == null) {
                this.renderer = new ExampleArmorRenderer();
            }
            this.renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
            return this.renderer;
            }
    });
}
```
