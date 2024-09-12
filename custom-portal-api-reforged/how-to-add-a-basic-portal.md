# How to add a basic portal

To add a basic portal in your mod, simply add

```java
// Begin building a custom portal
CustomPortalBuilder.beginPortal()

    // Set the frame block to Diamond Blocks
    // You can use a registered Block or a Resource Location here
    .frameBlock(Blocks.DIAMOND_BLOCK)
    
    // Set the item used to ignite the portal
    // Options include:
    // - Using an Ender Eye (as shown)
    // - Using water with lightWithWater()
    // - Using a custom fluid with lightWithFluid(MyFluids.CUSTOMFLUID)
    .lightWithItem(Items.ENDER_EYE)
    
    // (Optional) Set a forced size for the portal
    // Parameters are width and height
    .forcedSize(4, 5)
    
    // (Optional) Set a custom block for the portal itself
    // Replace MyBlocks.CUSTOMPORTALBLOCK with your custom portal block
    .customPortalBlock(MyBlocks.CUSTOMPORTALBLOCK)
    
    // (Optional) Configure the portal's return dimension
    // `onlyIgnitInReturnDim` specifies if the portal can only be ignited in the return dimension
    // 1.21+ requires ResourceLocation.parse or similar methods instead of new ResourceLocation
    .returnDim(new ResourceLocation("overworld"), onlyIgnitInReturnDim)
    
    // (Optional) Restrict portal ignition to the Overworld
    // Use this if you want the portal to be ignitable only in the Overworld
    .onlyLightInOverworld()
    
    // (Optional) Use the flat portal style, similar to the End Portal
    // Apply this style to make the portal's appearance flat
    .flatPortal()
    
    // Set the dimension to travel to when the portal is used
    // In this case, it sets the destination dimension to The End
    // 1.21+ requires ResourceLocation.parse or similar methods instead of new ResourceLocation
    .destDimID(new ResourceLocation("the_end"))
    
    // (Optional) Set the RGB color for the portal's tint
    // Customize the portal's appearance with a specific color
    .tintColor(45, 65, 101)
    
    // Register the custom portal with all the specified configurations
    .registerPortal();

```

to your mods constructor. That's it! You can see all the options [`CustomPortalBuilder`](https://github.com/AzureDoom/customportalapi-reforged/blob/1.20.1/src/main/java/net/kyrptonaught/customportalapi/api/CustomPortalBuilder.java) has to offer in its source.
