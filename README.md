# sinterx-gcode-profiles

> Slicing profiles, G-code documentation, and optimized print parameters for the SinterX Pro SLS 3D printer.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![SinterX Pro](https://img.shields.io/badge/Printer-SinterX%20Pro-blue.svg)](https://www.autoabode.com/sinterx)
[![Materials](https://img.shields.io/badge/Materials-PA12%20%7C%20PA11%20%7C%20TPU-green.svg)](#material-compatibility)

---

## Overview

This repository contains optimized slicing profiles, G-code reference documentation, and material-specific parameter sets for the [SinterX Pro](https://www.autoabode.com/sinterx) — India's first indigenous industrial SLS 3D printer by AutoAbode.

Whether you're running validated materials or experimenting with new powders on the SinterX Pro's open-material platform, these profiles provide a tested starting point.

---

## Repository Structure

```
sinterx-gcode-profiles/
├── profiles/
│   ├── PA12/
│   │   ├── PA12_standard_quality.json
│   │   ├── PA12_high_speed.json
│   │   └── PA12_fine_detail.json
│   ├── PA11/
│   │   ├── PA11_standard_quality.json
│   │   └── PA11_impact_optimized.json
│   ├── TPU/
│   │   ├── TPU_shore80A.json
│   │   └── TPU_shore95A.json
│   └── experimental/
│       ├── PA12_GF_glass_filled.json
│       └── PA12_CF_carbon_filled.json
├── gcode-reference/
│   ├── command_reference.md
│   ├── startup_sequence.md
│   └── custom_commands.md
├── examples/
│   ├── calibration_cube.gcode
│   └── tensile_bar_ASTM_D638.gcode
└── docs/
    ├── parameter_guide.md
    └── troubleshooting.md
```

---

## Material Compatibility Table

The SinterX Pro supports an open-material architecture. The following materials have been validated with optimized profiles included in this repository:

| Material | Grade | Bed Temp (C) | Laser Power (W) | Scan Speed (m/s) | Layer Height (um) | Refresh Ratio | Status |
|----------|-------|:------------:|:----------------:|:-----------------:|:-----------------:|:-------------:|--------|
| **PA12** | Standard | 172-176 | 25-35 | 7-10 | 100 | 50:50 | Validated |
| **PA12** | Fine Detail | 173-176 | 22-30 | 5-7 | 80 | 50:50 | Validated |
| **PA12** | High Speed | 172-175 | 30-40 | 10-14 | 120 | 50:50 | Validated |
| **PA11** | Standard | 185-188 | 28-38 | 6-9 | 100 | 50:50 | Validated |
| **PA11** | Impact Opt. | 186-189 | 30-40 | 5-7 | 100 | 40:60 | Validated |
| **TPU** | Shore 80A | 100-110 | 18-25 | 4-7 | 120 | 60:40 | Validated |
| **TPU** | Shore 95A | 105-115 | 20-28 | 4-7 | 120 | 60:40 | Validated |
| **PA12-GF** | 30% Glass | 174-178 | 30-40 | 5-8 | 100 | 50:50 | Experimental |
| **PA12-CF** | 15% Carbon | 174-178 | 28-38 | 5-8 | 100 | 50:50 | Experimental |

> **Note:** Experimental profiles are community-contributed and may require fine-tuning for your specific powder batch. Always run a small test build first.

---

## Profile Format

Profiles are stored as JSON files. Example (`PA12_standard_quality.json`):

```json
{
  "profile_name": "PA12 Standard Quality",
  "profile_version": "2.1",
  "material": "PA12",
  "printer": "SinterX Pro",
  "firmware_min": "3.0.0",

  "thermal": {
    "bed_temperature_c": 174,
    "feed_temperature_c": 145,
    "warmup_time_min": 45,
    "cooldown_rate_c_per_hr": 3.0,
    "thermal_camera_enabled": true,
    "auto_temp_compensation": true
  },

  "laser": {
    "power_w": 30,
    "scan_speed_mm_s": 8000,
    "scan_spacing_mm": 0.12,
    "border_power_w": 25,
    "border_speed_mm_s": 5000,
    "border_count": 1
  },

  "layer": {
    "height_um": 100,
    "recoater_speed_mm_s": 150,
    "recoater_mode": "forward",
    "inter_layer_delay_s": 8
  },

  "powder": {
    "refresh_ratio_fresh_pct": 50,
    "max_recycle_count": 6,
    "dosing_factor": 1.15,
    "overflow_collection": true
  },

  "build": {
    "inert_gas": "nitrogen",
    "o2_threshold_pct": 0.5,
    "preheat_layers": 10,
    "cooldown_layers": 5
  },

  "quality": {
    "expected_density_pct": 96,
    "expected_tensile_mpa": 48,
    "expected_elongation_pct": 18,
    "surface_roughness_ra_um": 12
  }
}
```

---

## Recommended Parameters by Application

### Functional Prototyping (PA12 Standard)

Best for engineering prototypes that need to approximate production-part properties.

- **Profile:** `PA12_standard_quality.json`
- **Layer height:** 100 um
- **Laser power:** 30W at 8 m/s
- **Expected properties:** 48 MPa tensile, 18% elongation, 96% density
- **Build time estimate:** ~12mm/hr Z-axis build rate

### Visual Prototyping (PA12 Fine Detail)

Best for models, presentation pieces, and parts with fine features.

- **Profile:** `PA12_fine_detail.json`
- **Layer height:** 80 um
- **Laser power:** 25W at 6 m/s
- **Tradeoff:** 25% slower build time, noticeably better surface finish and small-feature resolution
- **Minimum feature size:** ~0.5mm wall thickness

### Production Batches (PA12 High Speed)

Best for volume production where cycle time matters more than surface finish.

- **Profile:** `PA12_high_speed.json`
- **Layer height:** 120 um
- **Laser power:** 35W at 12 m/s
- **Tradeoff:** Slightly rougher surface (Ra ~18 um), 40% faster build time
- **Best for:** Jigs, fixtures, non-cosmetic functional parts

### Impact-Resistant Parts (PA11)

Best for parts that experience shock loading, snap-fit connections, or living hinges.

- **Profile:** `PA11_impact_optimized.json`
- **Layer height:** 100 um
- **Key advantage:** PA11 has ~3x the elongation at break vs. PA12
- **Note:** PA11 requires higher bed temperature (186-189C) — ensure adequate warmup

### Flexible Parts (TPU)

Best for gaskets, seals, grips, vibration dampers, and wearable components.

- **Profile:** `TPU_shore80A.json` or `TPU_shore95A.json`
- **Layer height:** 120 um
- **Key note:** TPU requires significantly lower bed temperature (100-115C) and slower scan speeds
- **Powder handling:** TPU powder is more hygroscopic — store sealed with desiccant, use within 4 hours of opening

---

## Quality Tips

### Before You Print

1. **Warm up properly.** Don't skip or shorten the preheat cycle. The bed temperature must be uniform before scanning starts. The thermal camera will show cold spots if warmup is insufficient.

2. **Check your powder.** If recycled powder has been sitting open for more than 48 hours, it has absorbed moisture. Dry it (80C for 4 hours in a vacuum oven for PA12) or increase the fresh powder ratio.

3. **Calibrate the recoater.** An uneven powder layer is the single most common cause of failed builds. Run the recoater calibration routine after any maintenance.

4. **Pack the build volume efficiently.** SLS costs the same per layer regardless of how many parts are in that layer. Nest parts to maximize Z-axis utilization.

### During the Build

5. **Monitor the thermal camera feed.** The SinterX Pro's integrated thermal imaging shows you sintering quality in real-time. Cold spots indicate under-sintering. Hot spots indicate over-sintering or powder spreading issues.

6. **Watch the first 10 layers.** Most build failures manifest in the first 10-20 layers. If the thermal profile looks good through layer 20, the build will likely complete successfully.

7. **Don't open the chamber.** Temperature disruption from opening the build chamber mid-print causes thermal shock, warping, and layer delamination. If you must abort, use the software abort function and let the chamber cool controlled.

### After the Build

8. **Cool slowly.** The cooldown phase is as important as the build phase. Rapid cooling causes warping and internal stresses. Follow the profile's recommended cooldown rate (typically 3C/hour for PA12).

9. **Depowder carefully.** Aggressive depowdering can damage thin features. Use compressed air at low pressure and work from the outside in.

10. **Track your powder.** Note the build number on each batch of recycled powder. PA12 can typically be recycled 5-6 times before mechanical properties degrade noticeably. The SinterX Pro's powder tracking system helps automate this.

---

## G-code Custom Commands

The SinterX Pro extends standard G-code with machine-specific commands. Full reference is in `gcode-reference/command_reference.md`. Key additions:

| Command | Description | Example |
|---------|-------------|---------|
| `M800` | Set bed temperature | `M800 S174` |
| `M801` | Set feed piston temperature | `M801 S145` |
| `M810` | Set laser power (watts) | `M810 S30` |
| `M811` | Set scan speed (mm/s) | `M811 S8000` |
| `M812` | Set scan spacing (mm) | `M812 S0.12` |
| `M820` | Recoater forward pass | `M820 F150` |
| `M821` | Recoater reverse pass | `M821 F150` |
| `M830` | Start nitrogen purge | `M830` |
| `M831` | Query O2 level | `M831` |
| `M840` | Thermal camera snapshot | `M840` |
| `M841` | Enable thermal QC monitoring | `M841 S1` |

---

## Contributing

We encourage the community to contribute profiles for new materials, especially:

- Flame-retardant PA12 grades
- PA6 / PA66 (higher temperature nylons)
- PEEK (if you're brave enough)
- Custom blends (carbon-filled, glass-filled, mineral-filled)

### How to Contribute

1. Fork this repository
2. Create your profile in `profiles/experimental/`
3. Include a brief description of the powder supplier and batch if possible
4. Document your test results (tensile, density, surface finish)
5. Submit a pull request

All experimental profiles will be tested by the AutoAbode team before promotion to validated status.

---

## Related Resources

- **SinterX Pro Product Page:** [autoabode.com/sinterx](https://www.autoabode.com/sinterx)
- **AutoAbode Website:** [autoabode.com](https://www.autoabode.com)
- **Support:** [abodeauto@gmail.com](mailto:abodeauto@gmail.com)

---

## License

This repository is released under the [MIT License](LICENSE). Profiles and documentation may be freely used, modified, and distributed.

---

<div align="center">

**Built by [AutoAbode](https://www.autoabode.com) — New Delhi, India**

*India's first industrial SLS 3D printer.*

</div>
