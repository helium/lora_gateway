add_executable(util_lbt_test
  src/util_lbt_test.c
  )
target_link_libraries(util_lbt_test
  LoraGW::loragw
  )
