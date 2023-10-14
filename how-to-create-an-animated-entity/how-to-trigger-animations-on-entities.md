# How to trigger Animations on Entities

## Setting up your controller

In your [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35) add [`triggerableAnim`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/animatable/SingletonGeoAnimatable.java#L84) to your controller like so

```java
@Override
public void registerControllers(ControllerRegistrar controllers) {
    controllers.add(
        new AnimationController<>(this, "baseAnim", event -> PlayState.CONTINUE)
        .triggerableAnim("death", RawAnimation.begin().thenPlayAndHold("death"));
}
```

You can add more triggers for each animation you want to call.&#x20;

## Calling the trigger

To call the trigger, you will simply need to call `triggerAnim` wherever you wish to use the trigger. Here is an example of calling on the `die` method of an Entity to play a death animation.

```java
@Override
public void die(DamageSource source) {
    this.triggerAnim("baseAnim", "death");
    super.die(source);
}
```
