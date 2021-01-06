cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(CPMLicenceTest)

include(../../cmake/CPM.cmake)

CPMAddPackage(NAME CPMLicences SOURCE_DIR ..)

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

create_write_licence_disclaimer_target(
  write-licences "${CMAKE_CURRENT_BINARY_DIR}/third_party.txt" "${CPM_PACKAGES}"
)
