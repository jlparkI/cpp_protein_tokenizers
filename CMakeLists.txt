set(CMAKE_VERBOSE_MAKEFILE on)
cmake_minimum_required(VERSION 3.18)
if (NOT SKBUILD)
  message(WARNING "\
  This CMake file is meant to be executed using 'scikit-build'. Running
  it directly will almost certainly not produce the desired result.")
endif()


find_package(Python 3.9 COMPONENTS Interpreter Development.Module REQUIRED)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

find_package(nanobind CONFIG REQUIRED)

nanobind_add_module(
  cpp_protein_encoders_ext

  # Target the stable ABI for Python 3.12+, which reduces
  # the number of binary wheels that must be built. This
  # does nothing on older Python versions
  STABLE_ABI

  NB_STATIC

  cpp_protein_encoders/cpp_ext/basic_encoders.cpp
  cpp_protein_encoders/cpp_ext/cpp_protein_encoders_ext.cpp
  cpp_protein_encoders/cpp_ext/subsmat_tokenizer.cpp
  cpp_protein_encoders/cpp_ext/utilities.cpp

)

install(TARGETS cpp_protein_encoders_ext LIBRARY DESTINATION ${SKBUILD_PROJECT_NAME})
