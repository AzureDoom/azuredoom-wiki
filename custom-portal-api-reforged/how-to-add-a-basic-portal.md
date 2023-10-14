# How to add a basic portal

To add a basic portal in your mod, simply call

```java
CustomPortalBuilder.beginPortal()
// Sets the frame block to Diamond blocks, 
// Can use registered Block or Resource Location
.frameBlock(Blocks.DIAMOND_BLOCK)
// Sets the lighting item to an Ender Eye
// Can use lightWithWater() instead to use water
// Can use lightWithFluid(MyFluids.CUSTOMFLUID) instead to use a custom Fluid
.lightWithItem(Items.ENDER_EYE)
// Not required, but can set a forced size of the portal
// Width, Height
.forcedSize(4, 5)
// Not required, but can set a custom Portal Block
.customPortalBlock(MyBlocks.CUSTOMPORTALBLOCK)
// Not required but can change the portals return dim
// onlyIgnitInReturnDim is a boolean to make the portal ignite only in the returnDim
.returnDim(new ResourceLocation("overworld"), onlyIgnitInReturnDim)
// Not required, but can make it so it can only be ignited in the OverWorld
.onlyLightInOverworld()
// Not required, but makes the portal shape use the End portal flat style instead
.flatPortal()
// Tells the portal to go the dim, in case The End
.destDimID(new ResourceLocation("the_end"))
// What RGB color to make the portal
.tintColor(45,65,101)
// Registers the portal
.registerPortal(); 
```

to your mods constructor. That's it! You can see all the options [`CustomPortalBuilder`](https://github.com/AzureDoom/customportalapi-reforged/blob/1.20.1/src/main/java/net/kyrptonaught/customportalapi/api/CustomPortalBuilder.java) has to offer in its source.
