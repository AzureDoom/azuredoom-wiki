# How to add GeoRenderLayers

To add GeoRenderLayers to any of AzureLib Geo renders, simply add them like so:&#x20;

```java
public class ExampleRenderer extends GeoEntityRenderer<ExampleEntity> {
	public ExampleRenderer(EntityRendererProvider.Context context) {
		super(context, new ExampleModel());
		// Add the glow layer
		addRenderLayer(new AutoGlowingGeoLayer<>(this));
	}
}
```

You can find the complete list of build-in Geo renders [`here`](https://github.com/AzureDoom/AzureLib/tree/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/layer).
