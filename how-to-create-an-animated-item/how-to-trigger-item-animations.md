# How to trigger Item Animations

## Setting up your Item

In your Item class, add [`SingletonGeoAnimatable.registerSyncedAnimatable`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/animatable/SingletonGeoAnimatable.java#L26) to your constructor. This is needed to sync the animations from the server to the clients.&#x20;

Next in your [`registerControllers`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/core/animatable/GeoAnimatable.java#L35) add [`triggerableAnim`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/animatable/SingletonGeoAnimatable.java#L84) to your controller like so

```java
@Override
public void registerControllers(ControllerRegistrar controllers) {
controllers.add(
    new AnimationController<>(this, "controllerName", event -> PlayState.CONTINUE)
        .triggerableAnim("animation", 
            RawAnimation.begin().then("animation", LoopType.PLAY_ONCE)));
}
```

You can add more triggers for each animation you need on your item.&#x20;

## Calling the trigger

To call the trigger, you will simply need to call `triggerAnim` wherever you wish to use the trigger, ensuring you are calling the server side only. Here is an example of calling on the onUseTick method of an item

<pre class="language-java"><code class="lang-java">@Override
public void onUseTick(Level level, LivingEntity entity, ItemStack stack, int count) {
    if (entity instanceof Player player)
<strong>        if (!level.isClientSide())
</strong><strong>            triggerAnim(player, GeoItem.getOrAssignId(stack, (ServerLevel) level), "controllerName", "animation");
</strong>}
</code></pre>
