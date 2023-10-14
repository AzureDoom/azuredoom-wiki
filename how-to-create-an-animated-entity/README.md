# How to create an Animated Entity

This page will not cover how to code an Entity from the ground up, only how to add the required Azurelib code for the animations and model to work.

## Needed Steps in Entity class

To start, you must implement [`GeoEntity`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/animatable/GeoEntity.java)

Override [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39) and [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35)

Instantiate a new [`AnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/instance/AnimatableInstanceCache.java) via [`AzureLibUtil.createInstanceCache(this)`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/util/AzureLibUtil.java) at the top of your entity class and return it in [`getAnimatableInstanceCache`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L42C39-L42C39)

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
		return event.setAndContinue(
			// If moving, play the walking animation
			event.isMoving() ? RawAnimation.begin().thenLoop("walking"): 
			// If not moving, play the idle animation
			RawAnimation.begin().thenLoop("idle"));
	})
	// Sets a Sound KeyFrame
	.setSoundKeyframeHandler(event -> {
		//Plays the step sound on the walk keyframes in an animation
		if (event.getKeyframeData().getSound().matches("walk"))
			if (level().isClientSide())
				level().playLocalSound(
					this.getX(), this.getY(), this.getZ(), 
					DoomSounds.PINKY_STEP, 
					SoundSource.HOSTILE, 0.25F, 1.0F, false);
	}));
}
```

## Creating the model class

Create a new class that extends [`GeoModel`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/model/GeoModel.java), then implement all the required methods:

<pre class="language-java"><code class="lang-java">public class ExampleEntityModel extends GeoModel&#x3C;ExampleEntity> {
// Models must be stored in assets/&#x3C;modid>/geo with subfolders supported inside the geo folder
<strong>	private static final ResourceLocation model = new ResourceLocation("yournamespace", "geo/yourmodel.geo.json");
</strong>// Textures must be stored in assets/&#x3C;modid>/geo with subfolders supported inside the textures folder
<strong>	private static final ResourceLocation texture = new ResourceLocation("yournamespace", "textures/&#x3C;modeltype>/yourtexture.png");
</strong>// Animations must be stored in assets/&#x3C;modid>/animations with subfolders supported inside the animations folder
	private static final ResourceLocation animation = new ResourceLocation("yournamespace", "animations/youranimation.animation.json");

	@Override
	public ResourceLocation getModelResource(ExampleEntity object) {
		return this.model;
	}

	@Override
	public ResourceLocation getTextureResource(ExampleEntity object) {
		return this.texture;
	}

	@Override
	public ResourceLocation getAnimationResource(ExampleEntity object) {
		return this.animation;
	}
}
</code></pre>

## Creating the renderer class

Create a class that extends [`GeoEntityRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/GeoEntityRenderer.java), then just set it like so:

```java
public class ExampleEntityRenderer extends GeoEntityRenderer<ExampleEntity> {
    public ExampleEntityRenderer(EntityRendererProvider.Context context) {
        super(context, new ExampleEntityModel());
    }
}
```

You will then need to register this class as you would any other renderer, depending on your modloader methods.
