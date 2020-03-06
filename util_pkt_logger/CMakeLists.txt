add_executable(util_pkt_logger
  src/parson.c
  src/util_pkt_logger.c
  )

target_include_directories(util_pkt_logger
  PRIVATE inc
  )

target_link_libraries(util_pkt_logger
  LoraGW::loragw
  )
