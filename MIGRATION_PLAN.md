# Zephyr 4.1 (HWMv2) Migration Plan for Eyelash Sofle

## Objective
Migrate the `eyelash_sofle` ZMK board definition from the legacy Zephyr structure to the new Hardware Model v2 (HWMv2) required by Zephyr 4.1+.

## 1. Directory Restructuring
- [x] **Move Board Directory:**
  - Move `boards/arm/eyelash_sofle/` to `boards/eyelash_sofle/`.
  - Remove empty `boards/arm/` directory.
- [x] **Cleanup Legacy Files:**
  - Remove `Kconfig.board` (replaced by unified `Kconfig`).
  - Remove `eyelash_sofle.yaml` (replaced by `board.yml`).

## 2. Implement HWMv2 Metadata (`board.yml`)
- [x] **Create `board.yml`:**
  - Define board name: `eyelash_sofle`.
  - Define Vendor: `custom`.
  - Define SoC: `nrf52840` (Note: **Not** `nrf52840_qiaa`).
  - Define Variants: `left`, `right`.

## 3. Kconfig Migration
- [x] **Create Unified `Kconfig`:**
  - Define `BOARD_EYELASH_SOFLE` common symbol.
  - Select SoC Series (`SOC_SERIES_NRF52X`) and specific SoC (`SOC_NRF52840`).
- [x] **Symbol Aliasing:**
  - Map generated HWMv2 symbols (`BOARD_EYELASH_SOFLE_NRF52840_LEFT`) to legacy internal symbols (`BOARD_EYELASH_SOFLE_LEFT`) to ensure `Kconfig.defconfig` logic works correctly.
- [x] **Path Resolution Fix:**
  - Use `rsource "Kconfig.defconfig"` instead of `source "Kconfig.defconfig"` to avoid path resolution errors (`$(BOARD_DIR)` is unreliable in some contexts).

## 4. Configuration Updates
- [x] **Update SoC Identifiers:**
  - Update `Kconfig`, `eyelash_sofle_left_defconfig`, and `eyelash_sofle_right_defconfig`.
  - Change `SOC_NRF52840_QIAA` \rightarrow `SOC_NRF52840`.
- [x] **Preserve Defaults:**
  - Ensure `Kconfig.defconfig` correctly applies `ZMK_SPLIT_ROLE_CENTRAL` and other settings based on the aliased symbols.

## 5. Build System & CI
- [x] **Update `west.yml`:**
  - Point `zmk` remote revision to `main` (Zephyr 4.1 support).
- [x] **Update `build.yaml`:**
  - Update build targets to use full HWMv2 syntax:
    - `eyelash_sofle/nrf52840/left`
    - `eyelash_sofle/nrf52840/right`
  - Update `nice_nano_v2` target to `nice_nano`.

## 6. Verification
- **Build Command:**
  ```bash
  west build -b eyelash_sofle/nrf52840/right
  west build -b eyelash_sofle/nrf52840/left
  ```
