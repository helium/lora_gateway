##########################################################################
# Project Setup                                                          #
##########################################################################
cmake_minimum_required(VERSION 3.0.2)
project(LoRaGatewayHAL
  VERSION 5.0.1
  LANGUAGES C
  )
set(CMAKE_C_STANDARD 99)
# Generate `compile_commands.json` in build directory
# Can be used with static analyzers and code-completion tools
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
include(CTest)

##########################################################################
# Static Analysis                                                        #
##########################################################################
find_program(CPPCHECK NAMES cppcheck)
if(CPPCHECK)
  message(STATUS "Found cppcheck: ${CPPCHECK}")
  set(CMAKE_C_CPPCHECK ${CPPCHECK}
    --enable=warning
    --inline-suppr
    --quiet
    --std=c${CMAKE_C_STANDARD}
    --suppress=missingIncludeSystem
    --suppress=unusedFunction
    --template=gcc
    )
endif()

##########################################################################
# Local                                                                  #
##########################################################################
add_subdirectory(libloragw)
add_subdirectory(util_lbt_test)
add_subdirectory(util_pkt_logger)
add_subdirectory(util_spectral_scan)
add_subdirectory(util_spi_stress)
add_subdirectory(util_tx_continuous)
add_subdirectory(util_tx_test)
