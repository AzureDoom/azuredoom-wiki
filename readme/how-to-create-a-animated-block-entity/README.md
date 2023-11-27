# How to create a Animated Block Entity

This page will not cover how to code a Block Entity from the ground up, only how to add the required Azurelib code for the animations and model to work.

## Needed Steps in Block Entity class

To start, you must implement [`GeoBlockEntity`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/animatable/GeoBlockEntity.java)

Override [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39) and [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35)

Instantiate a new [`AnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/instance/AnimatableInstanceCache.java) via [`AzureLibUtil.createInstanceCache(this)`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/util/AzureLibUtil.java) at the top of your entity class and return it in [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39)

```java
private final AnimatableInstanceCache cache = AzureLibUtil.createInstanceCache(this);

@Override
public AnimatableInstanceCache getAnimatableInstanceCache() {
    return cache;
}
```

Add any controllers you want for animations in [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35)&#x20;

```java
@Override
public void registerControllers(ControllerRegistrar controllers) {
	controllers.add(new AnimationController<>(this, "controllerName", 0, event ->
	{
		return event.setAndContinue( RawAnimation.begin().thenLoop("idle"));
	}));
}
```

## Needed Steps in the Block class

By default, Minecraft will search for a blockstate file in your assets folder to figure out how to render your block. Since the AzureLib renderer doesn't render based on a blockstate, we need to tell the game not to bother looking.

To do this, we need to override `getRenderShape` in our block and return the `ENTITYBLOCK_ANIMATED` type.

```java
@Override
public RenderShape getRenderShape(BlockState state) {
    return RenderShape.ENTITYBLOCK_ANIMATED;
}
```

## Creating the model class

Create a new class that extends [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/model/GeoModel.java), then implement all the required methods:

```java
public class ExampleBlockModel extends GeoModel<ExampleBlockEntity> {
// Models must be stored in assets/<modid>/geo with subfolders supported inside the geo folder
	private static final ResourceLocation model = new ResourceLocation("yournamespace", "geo/yourmodel.geo.json");
// Textures must be stored in assets/<modid>/textures with subfolders supported inside the textures folder
	private static final ResourceLocation texture = new ResourceLocation("yournamespace", "textures/<modeltype>/yourtexture.png");
// Animations must be stored in assets/<modid>/animations with subfolders supported inside the animations folder
	private static final ResourceLocation animation = new ResourceLocation("yournamespace", "animations/youranimation.animation.json");

	@Override
	public ResourceLocation getModelResource(ExampleBlockEntity object) {
		return this.model;
	}

	@Override
	public ResourceLocation getTextureResource(ExampleBlockEntity object) {
		return this.texture;
	}

	@Override
	public ResourceLocation getAnimationResource(ExampleBlockEntity object) {
		return this.animation;
	}
}
```

## Creating the renderer class

Create a class that extends [`GeoBlockRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/renderer/GeoBlockRenderer.java), then just set it like so:

```java
public class ExampleBlockEntityRenderer extends GeoBlockRenderer<ExampleBlockEntity>{
    public ExampleBlockEntityRenderer() {
        super(new ExampleBlockModel());
    }
}
```

You will then need to register this class as you would any other renderer, depending on your modloader methods.
