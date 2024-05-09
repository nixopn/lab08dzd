cmake_minimum_required(VERSION 3.4)
project(lab06)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(/solver_application)

include(CPack.cmake)
