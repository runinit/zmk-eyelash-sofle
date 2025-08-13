# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a custom ZMK firmware configuration for the Eyelash Sofle split keyboard - a 58-key mechanical keyboard with RGB underglow, per-key RGB backlighting, OLED displays, rotary encoders, and integrated pointing device support.

## Build Commands

### Local Development
ZMK firmware builds are handled through GitHub Actions automatically on push. For local building:

```bash
# Initialize west workspace (ZMK build system)
west init -l config/
west update
west zephyr-export

# Build firmware for both halves
west build -d build/left -b eyelash_sofle_left -- -DZMK_CONFIG="$(pwd)/config"
west build -d build/right -b eyelash_sofle_right -- -DZMK_CONFIG="$(pwd)/config"

# Build with ZMK Studio support (GUI configuration tool)
west build -d build/studio_left -b eyelash_sofle_left -- -DZMK_CONFIG="$(pwd)/config" -DCONFIG_ZMK_STUDIO=y -DCONFIG_ZMK_STUDIO_LOCKING=n
```

### Keymap Visualization
```bash
# Generate keymap SVG diagrams (requires keymap-drawer)
keymap -c keymap_drawer.config.yaml parse -z config/eyelash_sofle.keymap > keymap-drawer/eyelash_sofle.yaml
keymap -c keymap_drawer.config.yaml draw keymap-drawer/eyelash_sofle.yaml > keymap-drawer/eyelash_sofle.svg
```

## Architecture Overview

### Hardware Configuration
- **MCU**: Nordic nRF52840 (ARM Cortex-M4 with Bluetooth)
- **Layout**: 58-key split keyboard (29 keys per half)
- **Displays**: Nice!View OLED displays on both halves
- **RGB**: WS2812 underglow strips + per-key backlighting
- **Encoders**: Rotary encoder on left half (volume/scroll control)
- **Pointing Device**: Integrated mouse movement and scrolling
- **Wireless**: Bluetooth Low Energy with USB-C fallback

### Key ZMK Features Enabled
- **Multi-layer keymap** with 4 layers (Base, Upper, Lower, Adjust)
- **RGB underglow control** with automatic power management
- **Per-key backlighting** with brightness control
- **Mouse/pointing device support** with configurable acceleration
- **Bluetooth profiles** (up to 5 paired devices)
- **ZMK Studio support** for GUI-based configuration
- **Power management** with sleep timeout and low-power modes

### Keymap Structure
**Layer 0 (Base)**: Standard QWERTY with modifier access
**Layer 1 (Upper)**: Function keys, navigation, mouse controls, RGB settings
**Layer 2 (Lower)**: Bluetooth management, system controls, bootloader access
**Layer 3 (Adjust)**: Currently empty placeholder layer

### Key Behaviors
- **Mouse movement**: `&mmv MOVE_UP/DOWN/LEFT/RIGHT` for cursor control
- **Mouse scroll**: `&msc SCRL_UP/DOWN/LEFT/RIGHT` for scrolling
- **Mouse clicks**: `&mkp LCLK/RCLK/MCLK` for left/right/middle click
- **RGB control**: `&rgb_ug RGB_*` for underglow effects and brightness
- **Bluetooth**: `&bt BT_SEL 0-4` for device switching, `&bt BT_CLR` to clear

### File Structure
```
boards/arm/eyelash_sofle/          # Board definition files
├── eyelash_sofle.dtsi             # Main device tree (hardware config)
├── eyelash_sofle_left.dts         # Left half specific config
├── eyelash_sofle_right.dts        # Right half specific config
├── eyelash_sofle-layouts.dtsi     # Physical layout definition
├── eyelash_sofle.keymap           # Default keymap (board level)
├── eyelash_sofle.zmk.yml          # ZMK metadata
└── *_defconfig                    # Build configuration files

config/                            # User configuration
├── eyelash_sofle.keymap           # User keymap (overrides board default)
├── eyelash_sofle.conf             # ZMK feature configuration
└── west.yml                       # West manifest for ZMK modules

keymap-drawer/                     # Keymap visualization
├── *.yaml                         # Keymap data files
└── *.svg                          # Generated keymap diagrams

build.yaml                         # GitHub Actions build matrix
```

### Configuration Files Priority
1. `config/eyelash_sofle.keymap` - User keymap (highest priority)
2. `config/eyelash_sofle.conf` - Feature/behavior configuration  
3. `boards/arm/eyelash_sofle/eyelash_sofle.keymap` - Board default keymap
4. Device tree files (`.dtsi`, `.dts`) - Hardware definitions

### Development Workflow
1. **Modify keymap**: Edit `config/eyelash_sofle.keymap` for key assignments
2. **Adjust features**: Edit `config/eyelash_sofle.conf` for ZMK settings
3. **Hardware changes**: Modify device tree files in `boards/arm/eyelash_sofle/`
4. **Test build**: Push to trigger GitHub Actions or build locally
5. **Flash firmware**: Download .uf2 files from Actions and flash via bootloader

### Power Management
- Sleep timeout: 1 hour of inactivity
- RGB auto-off on idle and USB disconnect
- Aggressive debouncing (8ms) for battery efficiency
- External power control for RGB features

### Debugging
- USB logging disabled by default for power savings
- Enable with `CONFIG_ZMK_USB_LOGGING=y` in .conf file
- Access bootloader: Press reset button twice or use `&bootloader` key
- Factory reset: Use `&sys_reset` behavior or settings_reset shield