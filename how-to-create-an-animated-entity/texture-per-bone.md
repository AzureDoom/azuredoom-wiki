---
description: This page covers how to change the texture of a bone.
---

# Texture Per Bone

_**Due to the added performance overhead of this renderer, it is recommended to exercise discretion in its usage. Evaluate whether the advantages outweigh the performance cost for your specific requirements before employing it unnecessarily. It is recommended to use GeoRenderLayers instead where possible.**_

In your Entities render, call extend [`DynamicGeoEntityRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/DynamicGeoEntityRenderer.java) instead of [`GeoEntityRenderer`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/GeoEntityRenderer.java) and then call [`getTextureOverrideForBone`](https://github.com/AzureDoom/AzureLib/blob/1.20/Fabric/src/main/java/mod/azure/azurelib/renderer/DynamicGeoEntityRenderer.java#L51) to set the texture on that bone.

```java
public class PerBoneRenderer extends DynamicGeoEntityRenderer<PerBoneEntity> {
	private static final ResourceLocation BONE_TEXTURE =
		new ResourceLocation("modid", "textures/entity/perbonetexture.png");

	public PerBoneRenderer(EntityRendererProvider.Context renderManager) {
		super(renderManager, new PerBoneModel());
	}

	@Nullable
	@Override
	protected ResourceLocation getTextureOverrideForBone(GeoBone bone, PerBoneEntity animatable, float partialTick) {
		return bone.getName().equals("retextureme") ? BONE_TEXTURE : null;
	}
}
```
