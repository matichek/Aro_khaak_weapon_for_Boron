# X4 Foundations: Complete Turret Creation Guide

## Overview
This guide explains how to create custom turrets for X4 Foundations extensions, based on the structure used in the `boron_weapons` and `Aro_khaak_weapon_for_Boron` extensions.

## Required Files Structure
```
your_extension/
├── assets/
│   ├── fx/
│   │   └── weaponfx/
│   │       └── macros/
│   │           └── bullet_your_turret_macro.xml      # Custom bullet
│   │   └── gui/
│   │       └── textures/
│   │           └── upgrades/
│   │               └── turret_your_name_macro.gz     # Icon file
│   └── props/
│       └── weaponsystems/
│           └── faction/
│               ├── macros/
│               │   └── turret_your_turret_macro.xml  # Turret macro
│               └── turret_your_turret.xml            # Turret component
├── index/
│   ├── components.xml                                # Register component
│   └── macros.xml                                    # Register macros
├── libraries/
│   ├── wares.xml                                     # Economic data
│   └── icons.xml                                     # Icon registration
└── content.xml                                       # Extension metadata
```

## Step-by-Step Instructions

### 1. Create Turret Component File
**Location**: `assets/props/weaponsystems/[faction]/turret_[name].xml`

**Example**: `turret_bor_l_super_beam_01_mk1.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<components>
  <component name="turret_bor_l_super_beam_01_mk1" class="turret">
    <!-- Use existing geometry from base game or DLC -->
    <source geometry="extensions\ego_dlc_boron\assets\props\weaponsystems\boron\turret_bor_l_flak_01_mk1_data" />
    <layers>
      <layer />
    </layers>
    <connections>
      <!-- Copy connections from similar existing turret -->
      <connection name="container" tags="contents" value="0" />
      <connection name="position" tags="position" value="1" />
      <connection name="con_turret" tags="advanced combat component large turret " />
      <!-- ... more connections ... -->
      <connection name="con_laser_01" tags="laser " parent="part_gun">
        <offset>
          <position x="5.89e-07" y="0" z="35.83528" />
        </offset>
      </connection>
    </connections>
  </component>
</components>
```

### 2. Create Turret Macro File
**Location**: `assets/props/weaponsystems/[faction]/macros/turret_[name]_macro.xml`

**Example**: `turret_bor_l_super_beam_01_mk1_macro.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="turret_bor_l_super_beam_01_mk1_macro" class="turret">
    <component ref="turret_bor_l_super_beam_01_mk1" />
    <properties>
      <identification name="Boron Super Beam Turret" basename="Super Beam" shortname="BSB-T" makerrace="boron" description="Advanced Boron turret with devastating firepower" mk="1" />
      <bullet class="bullet_bor_turret_l_super_beam_01_mk1_macro" />
      <rotationspeed max="50" />
      <rotationacceleration max="100" />
      <reload rate="0.5" />
      <hull max="5000" threshold="0.2" integrated="0" />
      <effects>
        <sefx_damage_low ref="surfacemodule_damage_l_low" />
        <sefx_damage_medium ref="surfacemodule_damage_l_medium" />
        <sefx_damage_high ref="surfacemodule_damage_l_high" />
      </effects>
    </properties>
  </macro>
</macros>
```

### 3. Create Custom Bullet File
**Location**: `assets/fx/weaponfx/macros/bullet_[name]_macro.xml`

**Example**: `bullet_bor_turret_l_super_beam_01_mk1_macro.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="bullet_bor_turret_l_super_beam_01_mk1_macro" class="bullet">
    <!-- Use existing bullet component -->
    <component ref="bullet_ter_m_beam_01_mk2" />
    <properties>
      <ammunition value="1" reload="2.0" />
      <bullet speed="96000" lifetime="1.5" range="8000" amount="1" barrelamount="1" icon="weapon_beam_mk1" maxhits="1" ricochet="0" scale="1" attach="1" />
      <heat value="3000" />
      <reload rate="0.5" />
      <damage value="5000" repair="0" />
      <effects>
        <impact ref="impact_gen_l_laser_01_mk1" inside="impact_gen_l_laser_01_mk1_inside" />
        <bigobjectimpact ref="impact_gen_l_laser_01_mk1_bigobject" inside="impact_gen_l_laser_01_mk1_bigobject_inside" />
        <launch ref="muzzle_gen_l_laser_01_mk1" />
      </effects>
      <sounds>
        <firing ref="wpn_beam_gen_01" />
      </sounds>
      <weapon system="turret_longrange" />
    </properties>
  </macro>
</macros>
```

### 4. Register Component in Index
**File**: `index/components.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<diff>
 <add sel="/index">
  <entry name="turret_bor_l_super_beam_01_mk1" value="extensions\YourExtension\assets\props\weaponsystems\boron\turret_bor_l_super_beam_01_mk1"/>
 </add>
</diff>
```

### 5. Register Macros in Index
**File**: `index/macros.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<diff>
 <add sel="/index">
  <entry name="turret_bor_l_super_beam_01_mk1_macro" value="extensions\YourExtension\assets\props\weaponsystems\boron\macros\turret_bor_l_super_beam_01_mk1_macro"/>
  <entry name="bullet_bor_turret_l_super_beam_01_mk1_macro" value="extensions\YourExtension\assets\fx\weaponfx\macros\bullet_bor_turret_l_super_beam_01_mk1_macro"/>
 </add>
</diff>
```

### 6. Add Economic Data
**File**: `libraries/wares.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<diff>
  <add sel="/wares">
    <ware id="turret_bor_l_super_beam_01_mk1" name="Boron Super Beam Turret" description="Advanced Boron turret with devastating firepower" group="turrets" transport="equipment" volume="1" tags="equipment turret">
      <price min="150000" average="200000" max="250000" />
      <production time="15" amount="1" method="default" name="Universal">
        <primary>
          <ware ware="advancedelectronics" amount="20" />
          <ware ware="energycells" amount="100" />
          <ware ware="turretcomponents" amount="50" />
        </primary>
      </production>
      <component ref="turret_bor_l_super_beam_01_mk1_macro" amount="1"/>
      <restriction licence="generaluseequipment" />
      <use threshold="0" />
      <owner faction="boron" />
    </ware>
  </add>
</diff>
```

### 7. Create Icon File
1. **Copy existing icon**: Find a similar turret icon (`.gz` file) from another extension or base game
2. **Rename it**: `turret_bor_l_super_beam_01_mk1_macro.gz`
3. **Place in**: `assets/fx/gui/textures/upgrades/`

### 8. Register Icon
**File**: `libraries/icons.xml` (if not exists, create it)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<diff>
    <add sel="/icons">
        <icon name="upgrade_*" texture="extensions\YourExtension\assets\fx\gui\textures\upgrades\*.tga" height="256" width="256"/>
    </add>
</diff>
```

## Key Configuration Values

### Turret Performance
- **rotationspeed max**: How fast turret rotates (degrees/second)
- **rotationacceleration max**: How quickly it reaches max speed
- **reload rate**: Fire rate multiplier (lower = faster)
- **hull max**: Turret health points

### Bullet Performance
- **damage value**: Hull damage per shot
- **range**: Maximum effective range in meters
- **speed**: Projectile velocity
- **lifetime**: How long bullet exists (affects range)
- **reload rate**: Fire rate (lower = faster)
- **heat value**: Heat generated per shot

### Turret Classes by Size
- **Small (S)**: `weapon system="weapon_standard"`
- **Medium (M)**: `weapon system="turret_longrange"` or `turret_midrange`
- **Large (L)**: `weapon system="turret_longrange"`

### Common Component References
- **Small bullets**: `bullet_ter_s_beam_01_mk2`, `bullet_gen_s_laser_mk1`
- **Medium bullets**: `bullet_ter_m_beam_01_mk2`
- **Large bullets**: `bullet_ter_l_beam_01_mk1`

## Testing Checklist
1. ✅ All files created in correct locations
2. ✅ All entries added to index files
3. ✅ Component and macro names match exactly
4. ✅ Icon file exists and is properly named
5. ✅ Wares.xml contains economic data
6. ✅ Extension enabled in game
7. ✅ Game restarted after changes

## Common Issues
- **Turret doesn't appear**: Check index registration and icon file
- **Turret doesn't fire**: Verify bullet macro exists and is registered
- **Game crashes**: Check XML syntax and component references
- **No damage**: Verify bullet damage values and effects

## Example Folder Structure
Based on `boron_weapons` extension:
```
boron_weapons/
├── assets/fx/weaponfx/macros/
│   ├── bullet_bor_turret_l_beam_01_mk1_macro.xml
│   ├── bullet_bor_turret_m_beam_01_mk1_macro.xml
│   └── ...
├── assets/props/weaponsystems/boron/macros/
│   ├── turret_bor_l_beam_01_mk1_macro.xml
│   ├── turret_bor_m_beam_01_mk1_macro.xml
│   └── ...
├── assets/props/weaponsystems/boron/
│   ├── turret_bor_l_beam_01_mk1.xml
│   ├── turret_bor_m_beam_01_mk1.xml
│   └── ...
```

## Advanced Tips
1. **Copy existing turrets** as templates from `boron_weapons` or base game
2. **Use existing geometry** to avoid 3D modeling
3. **Test incrementally** - start with basic functionality, then enhance
4. **Keep naming consistent** throughout all files
5. **Use appropriate size classes** (S/M/L) for balance

---

**Note**: This guide is based on X4 Foundations modding structure. Always backup your files before making changes and test thoroughly in a separate save game.
