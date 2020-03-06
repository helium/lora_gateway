add_executable(util_spi_stress
  src/util_spi_stress.c
  )

target_include_directories(util_spi_stress
  PRIVATE inc
  )

target_link_libraries(util_spi_stress
  LoraGW::loragw
  )
