##############################################################################
# @file    CMakeLists.txt
# @author  GPM Application Team
#
##############################################################################
# @attention
#
# Copyright (c) 2025 STMicroelectronics.
# All rights reserved.
#
# This software is licensed under terms that can be found in the LICENSE file
# in the root directory of this software component.
# If no LICENSE file comes with this software, it is provided AS-IS.
#
##############################################################################

if (CONFIG_USE_STM32_MW_ISP)
  # bring stm32-mw-isp libraries, code and interface
  zephyr_include_directories(stm32-mw-isp/isp/Inc)

  zephyr_library_sources(stm32-mw-isp/isp/Src/isp_core.c
                         stm32-mw-isp/isp/Src/isp_awb_algo.c
                         stm32-mw-isp/isp/Src/isp_ae_algo.c
                         stm32-mw-isp/isp/Src/isp_algo.c
                         stm32-mw-isp/isp/Src/isp_cmd_parser.c
                         stm32-mw-isp/isp/Src/isp_services.c
                         stm32-mw-isp/isp/Src/isp_tool_com.c)

  # bring dcmipp driver / stm32-mw-isp glue code
  zephyr_include_directories(isp_wrapper/inc)
  zephyr_library_sources(isp_wrapper/src/isp_wrapper.c)
endif()
