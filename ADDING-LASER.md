# X4 Foundations: Complete Laser Weapon Creation Guide

## Overview
This guide explains how to create custom laser weapons for X4 Foundations extensions, based on the working implementation of the "Boron Khaak S Laser MK3" in the `Aro_khaak_weapon_for_Boron` extension.

## Required Files Structure
```
your_extension/
├── assets/
│   ├── fx/
│   │   ├── gui/
│   │   │   └── textures/
│   │   │       └── upgrades/
│   │   │           └── weapon_your_laser_macro.gz     # Icon file
│   │   └── weaponfx/
│   │       └── macros/
│   │           └── bullet_your_laser_macro.xml        # Custom bullet (optional)
│   └── props/
│       └── weaponsystems/
│           └── faction/
│               ├── macros/
│               │   └── weapon_your_laser_macro.xml    # Weapon macro
│               └── weapon_your_laser.xml              # Weapon component
├── index/
│   ├── components.xml                                 # Register component
│   └── macros.xml                                     # Register macros
├── libraries/
│   ├── wares.xml                                      # Economic data
│   └── icons.xml                                      # Icon registration
└── content.xml                                        # Extension metadata
```

## Step-by-Step Instructions

### 1. Create Weapon Component File
**Location**: `assets/props/weaponsystems/[faction]/weapon_[name].xml`

**Example**: `weapon_bor_s_laser_super_01_mk1.xml` (from Aro's mod)
```xml
<?xml version="1.0" encoding="utf-8"?>
<components>
  <component name="weapon_bor_s_laser_super_01_mk1" class="weapon">
    <!-- Use existing geometry from base game or DLC -->
    <source geometry="extensions\ego_dlc_boron\assets\props\weaponsystems\boron\weapon_bor_s_laser_01_mk1_data" />
    <layers>
      <layer />
    </layers>
    <connections>
      <connection name="position" tags="position" value="1" />
      <connection name="ConnectionForfx_main_decals" tags="detail_l forceoutline nocollision part ">
        <offset>
          <position x="-1.225e-07" y="-0.1606299" z="0.1834244" />
        </offset>
        <parts>
          <part name="fx_main_decals">
            <size>
              <max x="0.2554849" y="0.2930568" z="0.2754751" />
              <center x="3.73e-08" y="0.1214864" z="0.09351341" />
            </size>
          </part>
        </parts>
      </connection>
      <!-- Add connection points for laser firing -->
      <connection name="con_laser_01" tags="laser " parent="part_gun">
        <offset>
          <position x="1.5e-09" y="0.0895971" z="2.054415" />
        </offset>
      </connection>
      <connection name="con_laser_02" tags="laser " parent="part_gun">
        <offset>
          <position x="3e-09" y="-0.08880252" z="2.054415" />
        </offset>
      </connection>
    </connections>
  </component>
</components>
```

### 2. Create Weapon Macro File
**Location**: `assets/props/weaponsystems/[faction]/macros/weapon_[name]_macro.xml`

**Example**: `weapon_bor_s_laser_super_01_mk1_macro.xml` (successful implementation)
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="weapon_bor_s_laser_super_01_mk1_macro" class="weapon">
    <component ref="weapon_bor_s_laser_super_01_mk1" />
    <properties>
      <!-- Use direct text for unique identification -->
      <identification name="Boron Khaak S Laser MK3" basename="Super Laser" shortname="KHA" makerrace="boron" description="Advanced Boron laser weapon with enhanced range and damage output" mk="1" />
      
      <!-- Reference to custom bullet for enhanced performance -->
      <bullet class="bullet_kha_s_beam_super_01_macro" />
      
      <!-- Heat management for high-power weapon -->
      <heat overheat="25000" cooldelay="0.3" coolrate="5000" reenable="8000" />
      
      <!-- Weapon handling characteristics -->
      <rotationspeed max="60" />
      <rotationacceleration max="120" />
      <hull max="500" hittable="0" />
    </properties>
  </macro>
</macros>
```

### 3. Create Custom Bullet File (For Enhanced Performance)
**Location**: `assets/fx/weaponfx/macros/bullet_[name]_macro.xml`

**Example**: `bullet_kha_s_beam_super_01_macro.xml` (working implementation)
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="bullet_kha_s_beam_super_01_macro" class="bullet">
    <!-- Use existing bullet component for compatibility -->
    <component ref="bullet_ter_s_beam_01_mk2" />
    <properties>
      <!-- Ammunition system -->
      <ammunition value="1" reload="0.1" />
      
      <!-- Bullet physics and behavior -->
      <bullet speed="96000" lifetime="0.20" range="4000" amount="1" barrelamount="1" delay="0.1" icon="weapon_burst_mk2" timediff="0.05" angle="2" maxhits="1" ricochet="0" restitution="1" scale="1" attach="1" />
      
      <!-- Heat generation -->
      <heat value="2000" />
      
      <!-- Fire rate (lower = faster) -->
      <reload rate="10" />
      
      <!-- Damage output -->
      <damage value="25000" repair="0" />
      
      <!-- Visual and audio effects -->
      <effects>
        <impact ref="impact_gen_s_burst_01_mk1" />
        <launch ref="muzzle_gen_s_burst_01_mk1" />
      </effects>
      <sounds>
        <firing ref="wpn_beam_gen_01" />
      </sounds>
      
      <!-- Weapon system classification -->
      <weapon system="weapon_standard" />
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
  <entry name="weapon_bor_s_laser_super_01_mk1" value="extensions\Aro_khaak_weapon_for_Boron\assets\props\weaponsystems\boron\weapon_bor_s_laser_super_01_mk1"/>
 </add>
</diff>
```

### 5. Register Macros in Index
**File**: `index/macros.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<diff>
 <add sel="/index">
  <entry name="weapon_bor_s_laser_super_01_mk1_macro" value="extensions\Aro_khaak_weapon_for_Boron\assets\props\weaponsystems\boron\macros\weapon_bor_s_laser_super_01_mk1_macro"/>
  <entry name="bullet_kha_s_beam_super_01_macro" value="extensions\Aro_khaak_weapon_for_Boron\assets\fx\weaponfx\macros\bullet_kha_s_beam_super_01_macro"/>
 </add>
</diff>
```

### 6. Add Economic Data
**File**: `libraries/wares.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<diff>
  <add sel="/wares">
    <ware id="weapon_bor_s_laser_super_01_mk1" name="Boron Khaak S Laser MK3" description="Advanced Boron laser weapon with enhanced range and damage output" group="weapons" transport="equipment" volume="1" tags="equipment weapon">
      <price min="120000" average="150000" max="180000" />
      <production time="10" amount="1" method="default" name="Universal">
        <primary>
          <ware ware="advancedelectronics" amount="8" />
          <ware ware="energycells" amount="50" />
          <ware ware="weaponcomponents" amount="60" />
        </primary>
      </production>
      <component ref="weapon_bor_s_laser_super_01_mk1_macro" amount="1"/>
      <restriction licence="generaluseequipment" />
      <use threshold="0" />
      <owner faction="boron" />
    </ware>
  </add>
</diff>
```

### 7. Create Icon File
**Critical Step**: Copy an existing weapon icon and rename it
```bash
# Navigate to: assets/fx/gui/textures/upgrades/
# Copy existing file:
copy weapon_bor_s_laser_02_mk1_macro.gz weapon_bor_s_laser_super_01_mk1_macro.gz
```

### 8. Register Icon (if needed)
**File**: `libraries/icons.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<diff>
    <add sel="/icons">
        <icon name="upgrade_*" texture="extensions\Aro_khaak_weapon_for_Boron\assets\fx\gui\textures\upgrades\*.tga" height="256" width="256"/>
    </add>
</diff>
```

## Key Lessons from Aro's Super Laser Implementation

### What Made It Work
1. **Correct File Paths**: Using `assets/fx/weaponfx/macros/` for bullets (not `weaponeffects/bullet/macros/`)
2. **Proper Bullet Format**: Following exact boron_weapons structure with all required sections
3. **Both Registrations**: Registering BOTH weapon and bullet in `index/macros.xml`
4. **Icon File**: Must exist with exact matching filename
5. **Direct Text Names**: Avoiding ID conflicts by using direct text instead of `{20105,xxxx}` references
6. **Working Component Reference**: Using same component as working weapon (`weapon_bor_s_laser_02_mk1`)

### Common Issues and Solutions

#### Issue: Weapon Doesn't Appear
**Solutions**:
- ✅ Check icon file exists: `weapon_[name]_macro.gz`
- ✅ Verify macro registration in `index/macros.xml`
- ✅ Ensure unique name (no ID conflicts)

#### Issue: Weapon Doesn't Fire
**Solutions**:
- ✅ Check bullet file in correct path: `assets/fx/weaponfx/macros/`
- ✅ Verify bullet registration in `index/macros.xml`
- ✅ Use working bullet component reference

#### Issue: ID Conflicts
**Solutions**:
- ✅ Use direct text names instead of `{page,id}` references
- ✅ Check wares.xml for duplicate IDs
- ✅ Ensure unique weapon identifiers

## Performance Configuration Guide

### Damage Scaling
```xml
<!-- Normal S-class: ~300 damage -->
<damage value="317.7" repair="0" />

<!-- Enhanced: ~25x stronger -->
<damage value="25000" repair="0" />
```

### Range Configuration
```xml
<!-- Standard range -->
<bullet ... range="4000" ... />

<!-- Extended range -->
<bullet ... range="8000" ... />
```

### Fire Rate Control
```xml
<!-- Normal fire rate -->
<reload rate="6" />

<!-- Very fast fire rate -->
<reload rate="10" />
```

### Heat Management
```xml
<!-- Standard heat -->
<heat overheat="10000" cooldelay="1.13" coolrate="2000" reenable="7500" />

<!-- High-power heat -->
<heat overheat="25000" cooldelay="0.3" coolrate="5000" reenable="8000" />
```

## Weapon Size Classifications

### Small (S) Weapons
- **Component**: `bullet_ter_s_beam_01_mk2`
- **System**: `weapon_standard`
- **Typical Damage**: 200-500
- **Example**: Pulse Lasers, Burst Fire

### Medium (M) Weapons
- **Component**: `bullet_ter_m_beam_01_mk2`
- **System**: `weapon_standard`
- **Typical Damage**: 500-1500
- **Example**: Mining Lasers, Combat Beams

### Large (L) Weapons
- **Component**: `bullet_ter_l_beam_01_mk1`
- **System**: `weapon_standard`
- **Typical Damage**: 1500-5000+
- **Example**: Capital Ship Weapons

## Testing Checklist

### Before Testing
1. ✅ All files created in correct locations
2. ✅ Weapon macro and bullet macro registered in `index/macros.xml`
3. ✅ Component registered in `index/components.xml`
4. ✅ Icon file exists with exact filename match
5. ✅ Wares.xml contains economic data
6. ✅ Extension enabled in game

### In-Game Testing
1. ✅ Weapon appears in equipment selection
2. ✅ Weapon can be equipped on appropriate ships
3. ✅ Weapon fires when triggered
4. ✅ Damage values are as expected
5. ✅ Heat management works properly
6. ✅ Visual/audio effects display correctly

## Advanced Techniques

### Using Existing Components
Instead of creating new component files, reference existing ones:
```xml
<!-- Use working component reference -->
<component ref="weapon_bor_s_laser_02_mk1" />
```

### Custom Effects
Reference different visual effects:
```xml
<effects>
  <impact ref="impact_gen_l_laser_01_mk1" />
  <launch ref="muzzle_gen_l_laser_01_mk1" />
</effects>
```

### Faction Integration
Make weapons available to specific factions:
```xml
<owner faction="boron" />
<owner faction="argon" />
<owner faction="player" />
```

## File Templates

### Quick Start Template (Weapon Macro)
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="weapon_[faction]_[size]_[type]_[variant]_mk[level]_macro" class="weapon">
    <component ref="weapon_[existing_working_weapon]" />
    <properties>
      <identification name="Your Weapon Name" basename="Short Name" shortname="ABC" makerrace="faction" description="Your description" mk="1" />
      <bullet class="bullet_[your_bullet_name]_macro" />
      <heat overheat="15000" cooldelay="0.5" coolrate="3000" reenable="5000" />
      <rotationspeed max="60" />
      <rotationacceleration max="120" />
      <hull max="500" hittable="0" />
    </properties>
  </macro>
</macros>
```

### Quick Start Template (Bullet Macro)
```xml
<?xml version="1.0" encoding="utf-8"?>
<macros>
  <macro name="bullet_[your_name]_macro" class="bullet">
    <component ref="bullet_ter_s_beam_01_mk2" />
    <properties>
      <ammunition value="1" reload="0.2" />
      <bullet speed="96000" lifetime="0.15" range="5000" amount="1" barrelamount="1" delay="0.1" icon="weapon_burst_mk2" timediff="0.05" angle="2" maxhits="1" ricochet="0" restitution="1" scale="1" attach="1" />
      <heat value="1500" />
      <reload rate="8" />
      <damage value="10000" repair="0" />
      <effects>
        <impact ref="impact_gen_s_burst_01_mk1" />
        <launch ref="muzzle_gen_s_burst_01_mk1" />
      </effects>
      <sounds>
        <firing ref="wpn_beam_gen_01" />
      </sounds>
      <weapon system="weapon_standard" />
    </properties>
  </macro>
</macros>
```

---

**Note**: This guide is based on the successful implementation of the "Boron Khaak S Laser MK3" in the Aro_khaak_weapon_for_Boron extension. All examples are tested and working. Always backup your files and test in a separate save game.
