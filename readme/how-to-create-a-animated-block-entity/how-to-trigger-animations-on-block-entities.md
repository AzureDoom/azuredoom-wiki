# How to trigger Animations on Block Entities

## Setting up your controller

In your [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35) add [`triggerableAnim`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/animatable/SingletonGeoAnimatable.java#L84) to your controller like so

```java
@Override
public void registerControllers(ControllerRegistrar controllers) {
    controllers.add(
        new AnimationController<>(this, "baseAnim", event -> PlayState.CONTINUE)
        .triggerableAnim("anim", RawAnimation.begin().thenPlayAndHold("anim"));
}
```

You can add more triggers for each animation you want to call.&#x20;

## Calling the trigger

To call the trigger, you will simply need to call `triggerAnim` wherever you wish to use the trigger. Here is an example of calling on the `tick` method of a Block Entity to play an animation when applying effects.

```java
public static void tick(Level level, BlockPos pos, BlockState state, ExampleBlockEntity bEntity) {
    if (bEntity.getLevel().getGameTime() % 80L == 0L) {
	bEntity.applyEffects();
        this.triggerAnim("baseAnim", "anim");
    }
}
```
