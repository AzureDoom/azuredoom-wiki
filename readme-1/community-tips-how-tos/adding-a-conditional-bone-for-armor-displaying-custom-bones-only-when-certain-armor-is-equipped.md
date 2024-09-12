# Adding a Conditional Bone for Armor: Displaying Custom Bones Only When Certain Armor is Equipped

Here’s an example based on the [IgnisWizardArmorRenderer.java](https://github.com/AceTheEldritchKing/Cataclysm\_Spellbooks/blob/master/src/main/java/net/acetheeldritchking/cataclysm\_spellbooks/entity/render/armor/IgnisWizardArmorRenderer.java) from the Cataclysm Spellbooks repository. \
Credit to AceTheEldritchKing for this tip.

```java
/**
 * A renderer class for the ExampleArmorItem, which controls the visibility of armor bones
 * based on the equipped armor piece.
 */
public class ExampleArmorRenderer extends GeoArmorRenderer<ExampleArmorItem> {
    
    /**
     * Constructor for the ExampleArmorRenderer. Initializes the renderer with the ExampleArmorModel.
     */
    public ExampleArmorRenderer() {
        super(new ExampleArmorModel());
    }

    /**
     * Retrieves the bone associated with the armor legging's torso part.
     * This method uses the {@code model.getBone()} method to find the bone and returns {@code null} if not found.
     *
     * @return The {@code GeoBone} corresponding to the "newArmorBone", or {@code null} if it does not exist.
     */
    private GeoBone armorLeggingTorsoBone() {
        return this.model.getBone("newArmorBone").orElse(null);
    }

    /**
     * Adjusts the visibility of the armor bones depending on which equipment slot is currently filled.
     * The bones are initially hidden, and only those relevant to the equipped slot are made visible.
     *
     * @param currentSlot The equipment slot that is currently filled (e.g., head, chest, legs, or feet).
     */
    @Override
    protected void applyBoneVisibilityBySlot(EquipmentSlot currentSlot) {
        // Hide all bones initially
        this.setBoneVisible(getHeadBone(), false);
        this.setBoneVisible(getBodyBone(), false);
        this.setBoneVisible(getRightArmBone(), false);
        this.setBoneVisible(getLeftArmBone(), false);
        this.setBoneVisible(getRightLegBone(), false);
        this.setBoneVisible(getLeftLegBone(), false);
        this.setBoneVisible(getRightBootBone(), false);
        this.setBoneVisible(getLeftBootBone(), false);
        
        // Hide the legging torso bone initially
        this.setBoneVisible(armorLeggingTorsoBone(), false);

        // Make specific bones visible based on the equipped armor slot
        switch (currentSlot) {
            case HEAD -> this.setBoneVisible(getHeadBone(), true);
            case CHEST -> {
                this.setBoneVisible(getBodyBone(), true);
                this.setBoneVisible(getRightArmBone(), true);
                this.setBoneVisible(getLeftArmBone(), true);
            }
            case LEGS -> {
                // Make the legging torso bone visible when the legging armor is equiped
                this.setBoneVisible(armorLeggingTorsoBone(), true);
                this.setBoneVisible(getRightLegBone(), true);
                this.setBoneVisible(getLeftLegBone(), true);
            }
            case FEET -> {
                this.setBoneVisible(getRightBootBone(), true);
                this.setBoneVisible(getLeftBootBone(), true);
            }
        }
    }
}

```
