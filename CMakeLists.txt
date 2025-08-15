# SPDX-License-Identifier: MIT

if(CONFIG_ZMK_RGB_DUAL_BREATH)
  target_sources(app PRIVATE src/behaviors/behavior_rgb_dual_breath.c)
  target_include_directories(app PRIVATE include)
endif()