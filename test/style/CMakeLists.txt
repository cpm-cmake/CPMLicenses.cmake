cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(CPMLicenceTest)

include(../../cmake/CPM.cmake)

CPMAddPackage(
  NAME Format.cmake
  VERSION 1.6
  GITHUB_REPOSITORY TheLartians/Format.cmake
  OPTIONS "FORMAT_CHECK_CMAKE ON"
)
