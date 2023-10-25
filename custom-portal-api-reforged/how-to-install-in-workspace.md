# How to install in workspace

To add to your workspace, please add the following to your `build.gradle` :

```gradle
repositories {
    // The Maven with the mods source
    maven {url 'https://libs.azuredoom.com:4443/mods'}
}

dependencies {
    implementation fg.deobf("net.kyrptonaught.customportalapi:customportalapi-reforged:MODVERSION")
}
```
