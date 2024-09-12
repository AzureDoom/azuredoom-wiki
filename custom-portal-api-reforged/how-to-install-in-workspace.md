# How to install in workspace

To add to your workspace, please add the following to your `build.gradle` :

```gradle
repositories {
    // The Maven with the mods source
    maven {url 'https://libs.azuredoom.com:4443/mods'}
}

dependencies {
    // Forge 1.20.1
    implementation fg.deobf("net.kyrptonaught.customportalapi:customportalapi-reforged:MODVERSION")
    // NeoForge 1.20.4+
    implementation "net.kyrptonaught.customportalapi:customportalapi-reforged:MODVERSION"
}
```
