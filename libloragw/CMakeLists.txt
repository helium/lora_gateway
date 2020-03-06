add_library(loragw
  src/cursor.c
  src/loragw_aux.c
  src/loragw_fpga.c
  src/loragw_gps.c
  src/loragw_hal.c
  src/loragw_lbt.c
  src/loragw_radio.c
  src/loragw_reg.c
  src/loragw_spi.native.c
  src/packing.c
  )

target_include_directories(loragw
  PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/inc/loragw>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
  $<INSTALL_INTERFACE:include/loragw>
  PRIVATE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/inc>
  )

target_link_libraries(loragw
  PRIVATE m
  )

add_library(LoraGW::loragw ALIAS loragw)

##########################################################################
# Logging Options                                                        #
##########################################################################
option(DEBUG_AUX "Enable AUX logging")
option(DEBUG_SPI "Enable SPI logging")
option(DEBUG_REG "Enable REG logging")
option(DEBUG_HAL "Enable HAL logging")
option(DEBUG_LBT "Enable LBT logging")
option(DEBUG_GPS "Enable GPS logging")

##########################################################################
# Tests                                                                  #
##########################################################################
if(${BUILD_TESTING})
  # Tests are not run automatically (`make test`) due to the fact they
  # are still use hard-coded devices and paths that may not make sense.
  add_executable(test_loragw_cal tst/test_loragw_cal.c)
  target_link_libraries(test_loragw_cal PRIVATE LoraGW::loragw)

  add_executable(test_loragw_gps tst/test_loragw_gps.c)
  target_link_libraries(test_loragw_gps PRIVATE LoraGW::loragw)

  add_executable(test_loragw_hal tst/test_loragw_hal.c)
  target_link_libraries(test_loragw_hal PRIVATE LoraGW::loragw)

  add_executable(test_loragw_reg tst/test_loragw_reg.c)
  target_link_libraries(test_loragw_reg PRIVATE LoraGW::loragw)

  add_executable(test_loragw_spi tst/test_loragw_spi.c)
  target_link_libraries(test_loragw_spi PRIVATE LoraGW::loragw)
endif(${BUILD_TESTING})

##########################################################################
# Generate `config.h`                                                    #
##########################################################################
set(CONFIG_H_IN ${CMAKE_CURRENT_SOURCE_DIR}/config.h.in)
set(CONFIG_H_OUT ${CMAKE_CURRENT_BINARY_DIR}/config.h)
configure_file(${CONFIG_H_IN} ${CONFIG_H_OUT} @ONLY)

##########################################################################
# Install                                                                #
##########################################################################
include(GNUInstallDirs)

install(TARGETS loragw
  EXPORT LoraGWConfig
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )

export(TARGETS loragw
  NAMESPACE LoraGW::
  FILE "${CMAKE_CURRENT_BINARY_DIR}/LoraGWConfig.cmake"
  )

install(EXPORT LoraGWConfig
  DESTINATION ${CMAKE_INSTALL_DATADIR}/LoraGW/cmake
  NAMESPACE LoraGW::
  )

install(DIRECTORY inc/loragw DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(FILES ${CONFIG_H_OUT} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/loragw/)
