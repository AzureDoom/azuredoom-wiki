# How to add GeoRenderLayers

To add GeoRenderLayers to any of AzureLib Geo renders, simply add them like so:

```java
/**
 * Custom renderer class for the {@link ExampleEntity}.
 * This class handles rendering of the entity, including the addition of special layers like glowing effects.
 */
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {

    /**
     * Constructs a new {@link ExampleRenderer} with the specified rendering context.
     * Initializes the renderer with the {@link ExampleModel} and adds a glowing effect layer.
     *
     * @param context The {@link EntityRendererProvider.Context} used to manage rendering details like texture location and entity animations.
     */
    public ExampleRenderer(EntityRendererProvider.Context context) {
        super(context, new ExampleModel());
        // Adds a glowing layer to the entity's rendering.
        addRenderLayer(new AutoGlowingGeoLayer<>(this));
    }
}
```

You can find the complete list of built-in Geo renders [`here`](https://github.com/AzureDoom/AzureLib/tree/1.20/common/src/main/java/mod/azure/azurelib/renderer/layer).
