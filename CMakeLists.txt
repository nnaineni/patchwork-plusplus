cmake_minimum_required(VERSION 3.11)
project(PATCHWORK VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 14)
set(PYTHON_EXECUTABLE python3)
set(CMAKE_BUILD_TYPE Release)

if(CMAKE_VERSION VERSION_LESS "3.15")
  include("${CMAKE_CURRENT_SOURCE_DIR}/cmake/ListPrepend.cmake")
  list_prepend(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
else()
  list(PREPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
endif()

add_subdirectory(patchworkpp)

set(INCLUDE_PYTHON_WRAPPER ON CACHE BOOL "Build Python wrapper")
set(INCLUDE_EXAMPLES ON CACHE BOOL "Build examples")

if (INCLUDE_PYTHON_WRAPPER)
  message(STATUS "Building Python wrapper")
  find_package(Python3 REQUIRED COMPONENTS Interpreter Development.Module)

  include(FetchContent)
  FetchContent_Declare(
    pybind11
    GIT_REPOSITORY https://github.com/pybind/pybind11.git
    GIT_TAG        v2.12.0  # Updated to use the latest version
  )
  FetchContent_MakeAvailable(pybind11)

  add_subdirectory(python_wrapper)
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
    add_compile_options(-Wno-deprecated-declarations)
endif()
