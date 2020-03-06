add_executable(util_tx_continuous
  src/util_tx_continuous.c
  )

target_include_directories(util_tx_continuous
  PRIVATE inc
  )

target_link_libraries(util_tx_continuous
  LoraGW::loragw
  )
