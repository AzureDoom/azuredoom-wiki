# How to install in workspace

To add to your workspace, please add the following to your `build.gradle` :

```gradle
repositories {
    // The Maven with the mods source
    maven {url 'https://libs.azuredoom.com:4443/mods'}
}

dependencies {
    //Fabric or Quilt
    modImplementation "mod.azuredoom.bettercrawling:better-crawling-fabric-1.20.1:1.0.6"
		
    //NeoForge or Forge
    implementation fg.deobf("mod.azuredoom.bettercrawling:better-crawling-neoforge-1.20.1:1.0.6")
}
```
