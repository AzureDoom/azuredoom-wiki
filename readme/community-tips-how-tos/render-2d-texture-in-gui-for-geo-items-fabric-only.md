---
description: Thanks to MrSterner for this tip
---

# Render 2D texture in GUI for Geo Items (Fabric Only)

This guide will cover rendering an item with a 2d in the GUI and creating a custom renderer.

## Override `renderInGui` in Your Item Renderer

You need to override the `renderInGui` method for your item renderer and leave it empty. This method will be used in your `GeoItem#createRenderer` method.

```java
@Override
protected void renderInGui(ModelTransformationMode transformType, MatrixStack matrixStack, VertexConsumerProvider bufferSource, int packedLight, int packedOverlay) {
    // No custom rendering code here
}
```

## Create a Custom Renderer

Create a class that implements `BuiltinItemRendererRegistry.DynamicItemRenderer` and `IdentifiableResourceReloadListener`. This class will handle the rendering of your item in the GUI.

```java
public class TwoDItemRenderer implements BuiltinItemRendererRegistry.DynamicItemRenderer, IdentifiableResourceReloadListener {
    private final Identifier id;
    private final Identifier itemId;
    private ItemRenderer itemRenderer;
    private BakedModel inventoryModel;

    public TwoDItemRenderer(Identifier itemId) {
        this.id = new Identifier(itemId.getNamespace(), itemId.getPath() + "_renderer");
        this.itemId = itemId;
    }

    @Override
    public CompletableFuture<Void> reload(Synchronizer synchronizer, ResourceManager manager, Profiler prepareProfiler, Profiler applyProfiler, Executor prepareExecutor, Executor applyExecutor) {
        return synchronizer.whenPrepared(Unit.INSTANCE).thenRunAsync(() -> {
            applyProfiler.startTick();
            applyProfiler.push("listener");
            final MinecraftClient client = MinecraftClient.getInstance();
            this.itemRenderer = client.getItemRenderer();
            this.inventoryModel = client.getBakedModelManager().getModel(new ModelIdentifier(itemId.withPath(itemId.getPath() + "_gui"), "inventory"));
            applyProfiler.pop();
            applyProfiler.endTick();
        }, applyExecutor);
    }

    @Override
    public void render(ItemStack stack, ModelTransformationMode mode, MatrixStack matrices, VertexConsumerProvider vertexConsumers, int light, int overlay) {
        matrices.pop();
        matrices.push();
        if (mode != ModelTransformationMode.FIRST_PERSON_LEFT_HAND && mode != ModelTransformationMode.FIRST_PERSON_RIGHT_HAND && mode != ModelTransformationMode.THIRD_PERSON_LEFT_HAND && mode != ModelTransformationMode.THIRD_PERSON_RIGHT_HAND) {
            if (this.itemRenderer == null) {
                this.itemRenderer = MinecraftClient.getInstance().getItemRenderer();
            }
            if (this.inventoryModel == null) {
                this.inventoryModel = MinecraftClient.getInstance().getBakedModelManager().getModel(new ModelIdentifier(itemId.withPath(itemId.getPath() + "_gui"), "inventory"));
            }
            DiffuseLighting.enableGuiDepthLighting();
            renderItem(itemRenderer, stack, mode, false, matrices, vertexConsumers, CAVUtils.MAX_LIGHT, overlay, this.inventoryModel);
            DiffuseLighting.disableGuiDepthLighting();
        }
    }

    public void renderItem(ItemRenderer itemRenderer, ItemStack stack, ModelTransformationMode renderMode, boolean leftHanded, MatrixStack matrices, VertexConsumerProvider vertexConsumers, int light, int overlay, BakedModel model) {
        if (!stack.isEmpty()) {
            matrices.push();

            model.getTransformation().getTransformation(renderMode).apply(leftHanded, matrices);
            matrices.translate(-0.5F, -0.5F, -0.5F);
            if (!model.isBuiltin()) {
                RenderLayer renderLayer = RenderLayers.getItemLayer(stack, true);
                VertexConsumer vertexConsumer = ItemRenderer.getDirectItemGlintConsumer(vertexConsumers, renderLayer, true, stack.hasGlint());

                itemRenderer.renderBakedItemModel(model, stack, light, overlay, matrices, vertexConsumer);
            }

            matrices.pop();
        }
    }

    @Override
    public Identifier getFabricId() {
        return id;
    }
}
```

## Register the Renderer

Register your custom renderer and model reloading on the client side.

```java
static void registerRenderer(Item item) {
    Identifier itemId = Registries.ITEM.getId(item);
    var renderer = new TwoDItemRenderer(itemId);
    ResourceManagerHelper.get(ResourceType.CLIENT_RESOURCES).registerReloadListener((IdentifiableResourceReloadListener) renderer);
    BuiltinItemRendererRegistry.INSTANCE.register(item, (BuiltinItemRendererRegistry.DynamicItemRenderer) renderer);
    ModelLoadingRegistry.INSTANCE.registerModelProvider((manager, out) -> {
        out.accept(new ModelIdentifier(itemId.withPath(itemId.getPath() + "_handheld"), "inventory"));
        out.accept(new ModelIdentifier(itemId.withPath(itemId.getPath() + "_gui"), "inventory"));
    });
}
```

## Mixin to `ItemRenderer`

Use a mixin to override the `getModel` method in the `ItemRenderer` class to use your custom model.

```java
@Mixin(ItemRenderer.class)
public class ItemRendererMixin {
    @Shadow
    @Final
    private ItemModels models;

    @Inject(method = "getModel", at = @At("HEAD"), cancellable = true)
    private void cav$getHeldItemModel(ItemStack stack, World world, LivingEntity entity, int seed, CallbackInfoReturnable<BakedModel> cir) {
        if (stack.getItem() instanceof YourCoolGeoDualModelItem) {
            Identifier itemId = Registries.ITEM.getId(stack.getItem());
            var m = new ModelIdentifier(itemId.withPath(itemId.getPath() + "_handheld"), "inventory");
            BakedModel bakedModel = models.getModelManager().getModel(m);
            ClientWorld clientWorld = world instanceof ClientWorld ? (ClientWorld) world : null;
            BakedModel bakedModel2 = bakedModel.getOverrides().apply(bakedModel, stack, clientWorld, entity, seed);
            cir.setReturnValue(bakedModel2 == null ? this.models.getModelManager().getMissingModel() : bakedModel2);
        }
    }
}
```

## Creating Model JSON Files

Create the necessary model files for your item.

#### `models/item/blunderbuss_gui.json`

```json
{
  "parent": "item/generated",
  "textures": {
    "layer0": "modid:item/blunderbuss_item"
  }
}
```

`models/item/blunderbuss_handheld.json`

```json
{
  "parent": "builtin/entity",
  "texture_size": [
    ...
  ],
  "display": {
    ...
  }
}
```

_**Note: Replace**** ****`"modid:item/blunderbuss_item"`**** ****and other placeholder values with actual values relevant to your mod.**_
