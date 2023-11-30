# How to use the Plugin

{% hint style="danger" %}
Models created using the 1.0.4 version and older of the plugin will require converting once 1.0.5 is released. Please convert for now to Geckolib until the update is released.
{% endhint %}

## Creating the Model

To create a new Azurelib Model, go to `File` -> `New` -> `AzureLib Animated Model`

To choose your model type, go to `File` -> `AzureLib Model Settings` and select your object type. Existing types are: `Enity/Block/Item`, `Armor`, and `Block/Item`(Do not use this one, will be removed in the future)

If you have already created a Bedrock or Modded entity model, you can convert it to the Azurelib format by going to `File` -> `Convert Project` -> Select `AzureLib Animated Model`&#x20;

## Switching to the Animation tab

Once you have modeled and textured your model, you will want to click `Animate` next to the Paint tab. Animating in AzureLib is almost the same as animating in the Bedrock format. [Here is a great playlist on YouTube for animating in Blockbench](https://www.youtube.com/playlist?list=PLvULVkjBtg2TQWdJvyz-tpAqcwhIhctSP). Molang is also supported with the animations; see the Molang docs [here](https://bedrock.dev/docs/1.19.0.0/1.19.30.23/Molang#Math%20Functions) for more.

## Exporting the Model

After making your model, you must export several things.

1. The actual model (`File` -> `Export` -> `Export Azurelib .geo Model` -> save in the `geo` folder in the assets folder)
2. The texture (Right-click the texture in the `Textures` panel and `Save As`-> save in the `textures` folder in the assets folder)
3. The animation (`Animation` -> `Export Animations` -> save in the `animation` folder in the assets folder)
4. Export the display settings if you are doing a block or item model. (`File` -> `Export` -> `Export AzureLib Display Settings`)
