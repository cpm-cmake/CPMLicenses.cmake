cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(CPMlicenseTest)

include(../../cmake/CPM.cmake)

CPMAddPackage(NAME CPMLicenses.cmake SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/../..)

CPMAddPackage(
  NAME Catch2
  GITHUB_REPOSITORY catchorg/Catch2
  VERSION 2.5.0
  DOWNLOAD_ONLY YES
)

CPMAddPackage(
  NAME fmt
  GIT_TAG 6.1.2
  GITHUB_REPOSITORY fmtlib/fmt
  DOWNLOAD_ONLY YES
)

CPMAddPackage(
  NAME sol2
  URL https://github.com/ThePhD/sol2/archive/v3.2.1.zip
  VERSION 3.2.1
  DOWNLOAD_ONLY YES
)

cpm_licenses_create_disclaimer_target(
  write-licenses "${CMAKE_CURRENT_BINARY_DIR}/third_party.txt" "${CPM_PACKAGES}"
)
