# Interlocking Gears in FreeCAD

This guide explains how to create two gears where a smaller gear fits inside a larger one and both rotate together.

## Concept: Internal Gear (Ring) + Pinion

When you want a smaller gear to rotate **inside** a larger gear, you need:

1. **Internal Gear (Ring Gear)**: A gear with teeth pointing **inward**
2. **Pinion**: A smaller external gear that fits inside the ring

## Quick Start

### Using the Python Script

1. Open FreeCAD
2. Go to **View → Panels → Python console**
3. Run the script:
   ```python
   exec(open("/path/to/interlocking_gears.py").read())
   ```

The gears will be created automatically with default parameters.

### Customizing Parameters

Edit `interlocking_gears.py` and modify:

```python
# Module (must be the same for both gears!)
MODULE = 2.0  # mm

# Number of teeth on the internal (ring) gear
INTERNAL_GEAR_TEETH = 40

# Number of teeth on the pinion
PINION_TEETH = 15

# Pressure angle (standard: 20°)
PRESSURE_ANGLE = 20.0  # degrees

# Gear thickness
GEAR_THICKNESS = 10.0  # mm
```

## Method 2: Using FCGear Workbench (Recommended)

FCGear creates precise involute gear profiles, ideal for manufacturing.

### Installing FCGear:

1. Go to **Tools → Addon Manager**
2. Search for "FCGear"
3. Install and restart FreeCAD

### Creating the Internal Gear:

1. Switch to **FCGear** workbench
2. Click **Internal Involute Gear**
3. Set parameters:
   - **Teeth**: Number of teeth (e.g., 40)
   - **Module**: Module in mm (e.g., 2.0)
   - **Pressure Angle**: Pressure angle (e.g., 20°)
   - **Height**: Thickness (e.g., 10mm)

### Creating the Pinion:

1. Click **Involute Gear**
2. Use the **SAME module and pressure angle**:
   - **Teeth**: Fewer teeth (e.g., 15)
   - **Module**: 2.0 (must match!)
   - **Pressure Angle**: 20° (must match!)
   - **Height**: 10mm

### Positioning the Pinion:

Center distance formula:

```
Distance = Module × (Ring_Teeth - Pinion_Teeth) / 2
```

Example: `2.0 × (40 - 15) / 2 = 25 mm`

To position:
1. Select the pinion
2. Go to **View → Panels → Placement** (or press P)
3. Set **Position X** to the calculated value (e.g., 25 mm)

## Making Gears Rotate Together

### Using Spreadsheet:

1. Create a new spreadsheet: **Insert → Spreadsheet**
2. In cell A1, enter the pinion rotation angle (e.g., 0)
3. In cell B1, enter the formula: `=A1 * 15/40` (substitute your teeth counts)
4. Bind the pinion rotation to A1
5. Bind the ring gear rotation to B1
6. Change A1 to animate both gears!

### Gear Ratio:

For internal gears:

```
Ring_Rotation = Pinion_Rotation × (Pinion_Teeth / Ring_Teeth)
```

**IMPORTANT**: Unlike external gears, internal gears rotate in the **SAME direction**!

## Rules for Proper Meshing

For gears to mesh correctly:

1. ✅ **Same Module**: Both must have the same module
2. ✅ **Same Pressure Angle**: Usually 20°
3. ✅ **Different Teeth**: Ring must have more teeth than pinion
4. ✅ **Correct Distance**: Use the center distance formula
5. ✅ **Minimum Tooth Difference**: Ring should have at least 10-15 more teeth

## Troubleshooting

### Gears don't mesh:
- Verify modules are equal
- Recalculate center distance

### Teeth collide or excessive backlash:
- Adjust center distance
- Check pressure angle

### Internal gear looks inverted:
- This is normal! Teeth should point inward
- If using FCGear, use "Internal Involute Gear", not "Involute Gear"

## Exporting for Manufacturing

1. Select the gear
2. Go to **File → Export**
3. Choose format:
   - **.STEP** - For CNC machining
   - **.STL** - For 3D printing
   - **.DXF** - For laser cutting (export face)

## References

- [FCGear Documentation](https://wiki.freecadweb.org/FCGear_Workbench)
- [Gear Theory](https://en.wikipedia.org/wiki/Gear)
- [Internal Gears](https://en.wikipedia.org/wiki/Gear#Internal_gear)
