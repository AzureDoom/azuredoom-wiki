# Creating a config

## Creating the Config class

Create a new class like so:

```java
@Config(id = "yourmodid")
public class ModConfig {

}
```

Once created, simply register your fields for the configs you wish to use. These can only `public`, `non-final` and `instance` fields only.\
\
Supported datatypes include:

* `boolean`, `boolean[]`
* `char`
* `int`, `int[]`
* `long`, `long[]`
* `float`, `float[]`
* `double`, `double[]`
* `String`, `String[]`
* Enums
* Object\
  Objects are used for config nesting/splitting into multiple categories. Just include some `@`[`Configurable`](https://github.com/AzureDoom/AzureLib/blob/1.20/common/src/main/java/mod/azure/azurelib/config/Configurable.java) fields in the nested object instance.

An example of a config is

```java
@Config(id = MyMod.MODID)
public class ModConfig {

    @Configurable
    public boolean myBoolean = true;
    
    @Configurable
    public int myInt = 3;

    @Configurable
    public int[] myIntArray = {30, 20, 10, 5};

    @Configurable
    public CustomEnum myEnum = CustomEnum.SOME_VALUE;

    @Configurable
    public NestedObject myNest = new NestedObject();

    public static class NestedObject {
        @Configurable
        public double myDouble = 3.14159265359;
    }
}
```

## Registering the Config

### Forge

In your mods constructor, add the following:

```java
public static ModConfig config;

public YourMod() {
    config = AzureLibMod.registerConfig(ModConfig.class, 
        //you can use .json or .yaml formats
        ConfigFormats.json()).getConfigInstance();
}
```

### Fabric

In your mods Initializer class, add the following:

```java
public static ModConfig config;

@Override
public void onInitialize() {
    config = AzureLibMod.registerConfig(ModConfig.class, 
        //you can use .json or .yaml formats
        ConfigFormats.json()).getConfigInstance();
}
```

## Calling your Config fields

To call your use \<YourModInitializerClass>.config.\<config\_field> where you wish to use it!
