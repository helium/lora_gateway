add_executable(util_spectral_scan
  src/util_spectral_scan.c
  )

target_include_directories(util_spectral_scan
  PRIVATE inc
  )

target_link_libraries(util_spectral_scan
  LoraGW::loragw
  )
