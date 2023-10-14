# How to create Animated Armor

This page will not cover how to code Armor from the ground up, only how to add the required Azurelib code for the animations and model to work.

## Needed Steps in Armor Item

To start, you must implement [`GeoItem`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/animatable/GeoItem.java)

Override [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39) and [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35)

Instantiate a new [`AnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/instance/AnimatableInstanceCache.java) via [`AzureLibUtil.createInstanceCache(this)`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/util/AzureLibUtil.java) at the top of your entity class and return it in [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39) like so

```java
private final AnimatableInstanceCache cache = AzureLibUtil.createInstanceCache(this);

@Override
public AnimatableInstanceCache getAnimatableInstanceCache() {
    return cache;
}
```

Add any controllers you want for animations in [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35)&#x20;

```java
@Override
public void registerControllers(ControllerRegistrar controllers) {
	controllers.add(new AnimationController<>(this, "controllerName", 0, event ->
	{
		return event.setAndContinue(RawAnimation.begin().thenLoop("idle"));
	}));
}
```

## Creating the model class

Create a new class that extends [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/model/GeoModel.java), then implement all the required methods:

```java
public class ExampleArmorModel extends GeoModel<ExampleItem> {
// Models must be stored in assets/<modid>/geo with subfolders supported inside the geo folder
	private static final ResourceLocation model = new ResourceLocation("yournamespace", "geo/yourmodel.geo.json");
// Textures must be stored in assets/<modid>/textures with subfolders supported inside the textures folder
	private static final ResourceLocation texture = new ResourceLocation("yournamespace", "textures/<modeltype>/yourtexture.png");
// Animations must be stored in assets/<modid>/animations with subfolders supported inside the animations folder
	private static final ResourceLocation animation = new ResourceLocation("yournamespace", "animations/youranimation.animation.json");

	@Override
	public ResourceLocation getModelResource(ExampleItem object) {
		return this.model;
	}

	@Override
	public ResourceLocation getTextureResource(ExampleItem object) {
		return this.texture;
	}

	@Override
	public ResourceLocation getAnimationResource(ExampleItem object) {
		return this.animation;
	}
}
```

## Creating the renderer class

Create a class that extends [`GeoArmorRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/GeoArmorRenderer.java), then just set it like so:

```java
public class ExampleArmorRenderer extends GeoArmorRenderer<ExampleItem> {
    public ExampleArmorRenderer() {
        super(new ExampleArmorModel());
    }
}
```

## Registering the renderer

### Fabric/NeoForge/Forge 1.20.1+

In your Item class, add:&#x20;

```java
private final Supplier<Object> renderProvider = GeoItem.makeRenderer(this);

// Creates the render
@Override
public void createRenderer(Consumer<Object> consumer) {
	consumer.accept(new RenderProvider() {
		// Your render made above
		private ExampleArmorRenderer renderer;

		@Override
		public @NotNull HumanoidModel<LivingEntity> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<LivingEntity> original) {
			if (renderer == null)
				return new ExampleArmorRenderer();
			renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
			return this.renderer;
		}
	});
}

@Override
public Supplier<Object> getRenderProvider() {
	return renderProvider;
}
```

### Forge 1.19.4 and older

In your Item class, add:&#x20;

```java
@Override
public void initializeClient(Consumer<IClientItemExtensions> consumer) {
	consumer.accept(new IClientItemExtensions() {
		private ExampleItemRenderer renderer;

		@Override
		public @NotNull HumanoidModel<?> getHumanoidArmorModel(LivingEntity livingEntity, ItemStack itemStack, EquipmentSlot equipmentSlot, HumanoidModel<?> original) {
			if (renderer == null)
				return new ExampleItemRenderer();
			renderer.prepForRender(livingEntity, itemStack, equipmentSlot, original);
			return this.renderer;
		}
	});
}
```
