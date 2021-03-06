#
# Copyright 2013-2015 Ettus Research LLC
# Copyright 2018 Ettus Research, a National Instruments Company
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# This file included, use CMake directory variables
########################################################################

########################################################################
# Conditionally configure the USRP-E300 support
########################################################################
find_package(UDev)

if(ENABLE_E300)
    list(APPEND E300_SOURCES
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_impl.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_io_impl.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_fifo_config.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_sysfs_hooks.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_network.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_global_regs.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_spi.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_sensor_manager.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_i2c.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_eeprom_manager.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_common.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e300_remote_codec_ctrl.cpp
        ${CMAKE_CURRENT_SOURCE_DIR}/e3xx_radio_ctrl_impl.cpp
    )
    LIBUHD_APPEND_SOURCES(${E300_SOURCES})
    if(UDEV_FOUND AND NOT E300_FORCE_NETWORK)
        include_directories(${UDEV_INCLUDE_DIR})
        LIBUHD_APPEND_LIBS(${UDEV_LIBS})
        set_source_files_properties(
            ${E300_SOURCES}
            PROPERTIES COMPILE_DEFINITIONS "E300_NATIVE=1"
        )
    endif(UDEV_FOUND AND NOT E300_FORCE_NETWORK)

    if(ENABLE_GPSD)
        set_source_files_properties(
            ${CMAKE_CURRENT_SOURCE_DIR}/e300_impl.cpp
            ${CMAKE_CURRENT_SOURCE_DIR}/e300_impl.hpp
            ${CMAKE_CURRENT_SOURCE_DIR}/e3xx_radio_ctrl_impl.cpp
            PROPERTIES COMPILE_DEFINITIONS "E300_GPSD=1"
        )
    endif(ENABLE_GPSD)
endif(ENABLE_E300)
