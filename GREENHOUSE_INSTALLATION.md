# Greenhouse Sensor Installation Guide

This guide describes the installation of the GORGON soil temperature monitoring system in the greenhouse.

## System Overview

The greenhouse installation consists of:
- **3 GORGON sensors** (7-channel RTD each, 21 total measurement points)
- **1 Kestrel data logger** (central node)
- **Solar panel + battery** power system

Each GORGON sensor measures soil temperature at multiple depths using PT100 RTD probes in 4-wire configuration.

## Installation Layout

Refer to `WireMap.pdf` for the complete wiring diagram and sensor positions.

### Greenhouse Dimensions
- Length: 21' 10" (+2' for node area)
- Width: 14' 0"

### GORGON Sensor Locations

Three GORGON units are installed at different positions in the greenhouse:

| GORGON Unit | Node Wire Length | Position |
|-------------|------------------|----------|
| Unit 1 | 7' 0" | Near node |
| Unit 2 | 15' 6" | Mid-greenhouse |
| Unit 3 | 39' 6" | Far end |

### Sensor Depths

**Solo sensors**: Always installed at **3' 7"** depth

**Paired sensors** (x2): Installed at various depths per location (see wire map)

#### Location 1 (Left side)
| Wire Length | Quantity | Depth |
|-------------|----------|-------|
| 1' 5" | x2 | Shallow |
| 3' 0" | x2 | Medium |
| 4' 10" | x2 | Medium-deep |
| 8' 10" | x1 | Deep (solo) |

#### Location 2 (Right side, upper)
| Wire Length | Quantity | Depth |
|-------------|----------|-------|
| 1' 5" | x2 | Shallow |
| 3' 0" | x2 | Medium |
| 4' 10" | x2 | Medium-deep |
| 3' 10" | x1 | Solo (3' 7" depth) |

#### Location 3 (Right side, lower)
| Wire Length | Quantity | Depth |
|-------------|----------|-------|
| 3' 5" | x2 | Shallow-medium |
| 5' 0" | x2 | Medium |
| 6' 10" | x2 | Medium-deep |
| 3' 10" | x1 | Solo (3' 7" depth) |

## Installation Procedure

### 1. Prepare the Site

1. Mark sensor locations according to wire map
2. Clear any debris from installation points
3. Ensure soil is workable (not frozen or waterlogged)

### 2. Install RTD Probes

For each probe location:

1. **Drill pilot hole** to target depth using soil auger
2. **Insert RTD probe** carefully to avoid damaging sensor
3. **Backfill** around probe with native soil, compacting gently
4. **Route cable** along surface to GORGON enclosure
5. **Protect cables** with conduit or burial where needed

**Important**:
- Maintain proper 4-wire connection integrity
- Avoid sharp bends in cables
- Keep cable runs away from foot traffic areas

### 3. Connect GORGON Units

1. Open GORGON enclosure
2. Connect RTD cables to corresponding channels (1-7)
3. Verify connections are secure
4. Close and seal enclosure

### 4. Connect to Kestrel Logger

1. Run SDI-12 cable from each GORGON to Kestrel node
2. Connect using cable lengths specified in wire map:
   - Unit 1: 7' 0"
   - Unit 2: 15' 6"
   - Unit 3: 39' 6"
3. Connect power cables from Kestrel to each GORGON

### 5. Install Power System

1. Mount solar panel in location with good sun exposure
2. Connect solar panel to charge controller
3. Connect battery to charge controller
4. Connect Kestrel to battery power

### 6. Verify Installation

1. Power on system
2. Check SDI-12 communication with each GORGON:
   ```
   # From Kestrel or serial terminal
   0I!    # Should respond with GORGON ID
   1I!    # Second GORGON
   2I!    # Third GORGON
   ```
3. Take test measurements from each channel:
   ```
   0M1!   # Measure channel 1 on GORGON 0
   0D0!   # Retrieve data
   ```
4. Verify all 21 channels report reasonable temperatures

## SDI-12 Addressing

Each GORGON must have a unique SDI-12 address:

| GORGON Unit | SDI-12 Address |
|-------------|----------------|
| Unit 1 | 0 |
| Unit 2 | 1 |
| Unit 3 | 2 |

To change address, connect via UART and use NVM commands (see firmware documentation).

## Maintenance

### Regular Checks
- Inspect cable connections monthly
- Check enclosure seals for moisture intrusion
- Verify solar panel is clean and unobstructed
- Review data for sensor failures or drift

### Troubleshooting

| Issue | Possible Cause | Solution |
|-------|----------------|----------|
| No response from GORGON | Power issue | Check power connections, battery voltage |
| Single channel reads FAIL | Broken RTD wire | Check probe connections, replace if damaged |
| Erratic readings | Poor connection | Clean and reseat cable connections |
| All readings offset | Calibration drift | Recalibrate using precision resistors |

## Data Collection

The Kestrel logger polls each GORGON sensor at configured intervals and stores data to SD card.

### Measurement Commands (per GORGON)
- `M1` - `M7`: RTD channels 1-7 (soil temperatures)
- `M8`: GORGON internal temperature (Pico)
- `M9`: ADC internal temperature

### Data Retrieval
1. Remove SD card from Kestrel
2. Copy data files to computer
3. Use Python analysis tools in firmware repo (`tools/sdi12-logger/`)

## Reference Documents

| Document | Location |
|----------|----------|
| Wire Map | `WireMap.pdf` (this repo) |
| GORGON Hardware | `README.md` (this repo) |
| Firmware & Tools | https://github.com/RTGS-Lab/Project-Gorgon-Firmware |
| Calibration Guide | Firmware repo: `calibration/CALIBRATION_GUIDE.md` |

## Contact

For questions about this installation, refer to the project handoff documentation in the firmware repository.
