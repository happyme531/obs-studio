cmake_minimum_required(VERSION 3.28...3.30)

if(CMAKE_VS_PLATFORM_NAME STREQUAL ARM64)
  add_subdirectory(encoder)
endif()
add_subdirectory(capture)
