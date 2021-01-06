cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(CPMLicenceTest)

include(../../cmake/CPM.cmake)

CPMAddPackage(NAME CPMLicences.cmake SOURCE_DIR ..)

CPMAddPackage(
  NAME Catch2
  GITHUB_REPOSITORY catchorg/Catch2
  VERSION 2.5.0
)

CPMAddPackage(
  NAME fmt
  GIT_TAG 6.1.2
  GITHUB_REPOSITORY fmtlib/fmt
)

cpm_licences_create_disclaimer_target(
  write-licences "${CMAKE_CURRENT_BINARY_DIR}/third_party.txt" "${CPM_PACKAGES}"
)
