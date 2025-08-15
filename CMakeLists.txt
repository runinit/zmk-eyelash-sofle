# SPDX-License-Identifier: MIT

if(CONFIG_ZMK_RGB_DUAL_BREATH)
  target_sources(zmk__base PRIVATE src/behaviors/behavior_rgb_dual_breath.c)
  target_include_directories(zmk__base PRIVATE include)
endif()