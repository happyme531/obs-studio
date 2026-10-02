cmake_minimum_required(VERSION 3.28...3.30)

add_core_module(encoder PLATFORMS WINDOWS ARCHITECTURES ARM64)
add_subdirectory(capture)
