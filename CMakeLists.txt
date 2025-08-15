# SPDX-License-Identifier: MIT

# Make include directory available globally for devicetree preprocessing
list(APPEND ZEPHYR_DTC_FLAGS "-I${CMAKE_CURRENT_SOURCE_DIR}/include")

if(CONFIG_ZMK_RGB_DUAL_BREATH)
  target_sources(app PRIVATE src/behaviors/behavior_rgb_dual_breath.c)
  target_include_directories(app PRIVATE include)
endif()