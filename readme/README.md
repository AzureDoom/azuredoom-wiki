---
description: Minecraft Animation Engine and helper library.
cover: ../.gitbook/assets/BH_AL_header.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# AzureLib

AzureLib represents a branch derived from Geckolib 4.x, serving as an animation engine tailored for Minecraft Mods. It boasts various features, including support for intricate 3D keyframe-driven animations, over 30 different easing functions, concurrent animation capabilities, sound and particle keyframes, event-based keyframes, and numerous other functionalities. Currently, I'll focus on maintaining and supporting AzureLib; no help will be given to Geckolib.

This library is compatible with the following Minecraft versions:

* Forge: 1.16.5, 1.17.1, 1.18.2, 1.19.2, 1.19.4, and 1.20.1.
* NeoForge: 1.20.1, 1.20.4, 1.20.6
* Fabric: 1.16.5, 1.17.1, 1.18.2, 1.19.2, 1.19.4, 1.20.1, 1.20.4 and 1.20.6.

In my fork, I've removed the example content and introduced some additional features, including:

1. [A built-in Configuration library for Fabric 1.19.4+, Forge 1.20.1, NeoForge 1.20.1+](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/config/TestingConfig.java).
2. [Customized navigation to address the issue of mobs with large hitboxes spinning](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/ai/pathing/AzureNavigation.java).
3. [An exclusive light source block that persists for half a tick.](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/entities/TickingLightBlock.java) (I utilize this for my various mods to illuminate projectiles like flares or simulate a muzzle flash.)
4. [My base gun code](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/items/BaseGunItem.java).
5. [My mob growth system](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/helper/Growable.java), as seen in [`Gigeresque`](https://modrinth.com/mod/gigeresque) and [`Aftershock`](https://modrinth.com/mod/aftershock), to transform entities into different entities.
6. [A feature enabling loading another file within the same animation file (developed by the CQR developer)](how-to-use-the-inclusion-feature-in-animation-files.md).
7. [A unique vibration system that eliminates particle effects, allowing any mob to emit vibrations similar to the warden's](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/helper/AzureVibrationUser.java).

These additions enhance the capabilities of AzureLib for various creative applications.

## Installation

To add to your workspace, please add the following to your `build.gradle` :

<pre class="language-gradle"><code class="lang-gradle">repositories {
    // The Maven with the mods source
    maven {url 'https://libs.azuredoom.com:4443/mods'}
    // Needed for Fabric only at the moment
    maven { url "https://maven.terraformersmc.com/releases" }
}

dependencies {
    //Common 1.20.1+ Latest Only
    compileOnly "mod.azure.azurelib:azurelib-common-MCVERSION:MODVERSION"
  
    //Fabric or Quilt and older
    modImplementation "mod.azure.azurelib:azurelib-fabric-MCVERSION:MODVERSION"
    modApi "com.terraformersmc:modmenu:VERSION" // Fabric bug is requiring this

    //Forge 1.20.1 and older (Forge is no longer supported)
    implementation fg.deobf("mod.azure.azurelib:azurelib-forge-MCVERSION:MODVERSION")
  
    //NeoForge 1.20.1
    implementation fg.deobf("mod.azure.azurelib:azurelib-neo-MCVER:MODVER")
  
    //NeoForge 1.20.4+
    implementation "mod.azure.azurelib:azurelib-neo-MCVER:MODVER"
}
</code></pre>

To install the AzureLIb Blockbench Plugin, follow these steps:

1. Start Blockbench and open the file `File` menu in the top left corner.
2. Select `Plugins...` to open the built-in plugin browser.
3. Make sure you are in the `Available` tab, then search for the plugin: `AzureLib Animator`.
4. Once you have found it, press the `Install` button on the right-hand side.

### Support <a href="#support" id="support"></a>

If you have questions or need help getting up and running with questions, feel free to join my [Discord](https://discord.azuredoom.com/)!

### License <a href="#license" id="license"></a>

[MIT](https://github.com/AzureDoom/AzureLib/blob/1.20/LICENSE)
