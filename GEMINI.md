# Project Overview

This is a ZMK firmware configuration project for the **Eyelash Sofle**, a split ergonomic keyboard based on the Sofle design. It supports features like RGB underglow, encoders, and mouse emulation. The project is set up to build firmware for both left and right halves of the keyboard using the ZMK framework.

## Key Features
- **Split Keyboard:** Support for left and right distinct halves.
- **RGB Underglow:** Configurable LED lighting (WS2812).
- **Encoders:** Support for rotary encoders (EC11).
- **Pointing Device:** Mouse key emulation enabled.
- **ZMK Studio:** Support for ZMK Studio on the left half.

# Building and Running

This project uses the standard ZMK `west` build system.

## Prerequisites
- Docker (recommended for local builds) or a local Zephyr/ZMK development environment.

## Build Commands
Using `west` (assuming environment is set up):

```bash
# Build left half
west build -b eyelash_sofle_left -- -DSHIELD=nice_view

# Build right half
west build -b eyelash_sofle_right -- -DSHIELD=nice_view
```

*Note: The `build.yaml` suggests usage of `nice_view` shields.*

## CI/CD
The project includes GitHub Actions workflows (`.github/workflows/build.yml`) to automatically build firmware artifacts on push/pull_request.

# Key Files & Directories

*   `config/eyelash_sofle.keymap`: Defines the key layout, layers, and behaviors (macros, combos).
*   `config/eyelash_sofle.conf`: ZMK Kconfig options (enabling features like Deep Sleep, RGB, Mouse).
*   `build.yaml`: Configuration for the ZMK build matrix (used by CI).
*   `boards/`: Custom board definitions for the Eyelash Sofle.
*   `keymap-drawer/`: Configuration and generated images for visualizing the keymap.

# Development Conventions

*   **Keymap Editing:** Edit `config/eyelash_sofle.keymap` to change key bindings.
*   **Feature Configuration:** Edit `config/eyelash_sofle.conf` to enable/disable hardware features (RGB, Sleep, etc.).
*   **Formatting:** Follow DeviceTree syntax for `.keymap` and `.dts` files.
