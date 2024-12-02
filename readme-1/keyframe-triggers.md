# Keyframe Triggers

## Sound Keyframes

To manage sound keyframe callbacks, attach an [`SoundKeyframeHandler`](https://github.com/AzureDoom/AzureLib/blob/1.21-dev/common/src/main/java/mod/azure/azurelib/core/animation/AnimationController.java#L146-L156) instance to your `AnimationController` using the `setSoundKeyframeHandler` method. This handler will be invoked at the specified time defined by the keyframe in the animation JSON.

### Example

```java
    @Override
    public void registerControllers(AnimatableManager.ControllerRegistrar controllers) {
        controllers.add(
            new AnimationController<>(this,"main_controller",5, state -> PlayState.STOP)
            .setSoundKeyframeHandler(event -> {
                if (event.getKeyframeData().getSound().matches("customSoundkey") && state.getAnimatable().level().isClientSide)
                    state.getAnimatable().level().playLocalSound(
                            this.getX(),
                            this.getY(),
                            this.getZ(),
                            CustomSounds.YOUR_CUSTOM_SOUND,
                            SoundSource.HOSTILE,
                            1.0F, // Volume
                            1.0F, // Pitch
                            true //Distance Delay
                        );
            })
        );
    }
```

## Particle Keyframes

To handle particle keyframe callbacks, attach an [`ParticleKeyframeHandler`](https://github.com/AzureDoom/AzureLib/blob/1.21-dev/common/src/main/java/mod/azure/azurelib/core/animation/AnimationController.java#L158-L168) instance to your `AnimationController` using the `setParticleKeyframeHandler` method. This handler will be triggered at the specified time defined by the keyframe in the animation JSON.

### Example

```java
    @Override
    public void registerControllers(AnimatableManager.ControllerRegistrar controllers) {
        controllers.add(
            new AnimationController<>(this, "main_controller", 0, state -> PlayState.STOP)
                .setParticleKeyframeHandler(state -> {
                    if (event.getKeyframeData().getSound().matches("customSoundkey"))
                        state.getAnimatable().level().addParticle(ParticleTypes.ASH, 1, 1, 1, 1, 1, 1);
                })
        );
    }
```

## Custom Instruction Keyframes

Custom instructions help perform actions on your entity unrelated to sound or particles at specific keyframe timings.&#x20;

To handle custom keyframe callbacks, add an [`ICustomInstructionListener`](https://github.com/AzureDoom/AzureLib/blob/1.21-dev/common/src/main/java/mod/azure/azurelib/core/animation/AnimationController.java#L170-L182) instance to your `AnimationController` using the `setCustomInstructionKeyframeHandler` method. This handler will be triggered at the specified time defined by the keyframe in the animation JSON.

### Example

<pre class="language-java"><code class="lang-java"><strong>    @Override
</strong>    public void registerControllers(AnimatableManager.ControllerRegistrar controllers) {
        controllers.add(
            new AnimationController&#x3C;>(this, "main_controller", 0, state -> PlayState.STOP)
                .setCustomInstructionKeyframeHandler(state -> {
                    // Use helper method to avoid client-code in common class
                    Player player = ClientUtils.getClientPlayer();

                    if (event.getKeyframeData().getSound().matches("customSoundkey") &#x26;&#x26; player != null)
                        player.displayClientMessage(Component.literal("This is a custom message"), true);
                })
        );
    }
</code></pre>
