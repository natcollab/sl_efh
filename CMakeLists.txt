#[===============================================================================[.rst
# Slefh/CMakeLists.txt
#
# Slefh library.
# ^^^^^^^^^^^^
#
# Imported targets in global scope:
#
#   Slefh::Slefh
#
#]===============================================================================]

# Setup minimum required version of cmake, policy
# This command should be the first command in the project
cmake_minimum_required(VERSION 3.13...3.16)

# Setup Project name and version
# CMakeTest_VERSION and PROJECT_VERSION variables are set
project(Slefh VERSION 1.0
  DESCRIPTION "Interface to Slefh library"
  LANGUAGES CXX
  )

#=================================================================================

# Target: Slefh
add_library(Slefh INTERFACE)
add_library(Slefh::Slefh ALIAS Slefh)

set(Slefh_INCLUDE_INSTALL_DIR include/Slefh)
set(Slefh_LIB_INSTALL_DIR lib/Slefh)
set(Slefh_CMAKE_INSTALL_DIR lib/cmake/Slefh)

target_include_directories(Slefh
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
             $<INSTALL_INTERFACE:${Slefh_INCLUDE_INSTALL_DIR}>
  )

target_link_libraries(Slefh
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/libslefh.so>
             $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/${Slefh_LIB_INSTALL_DIR}/libslefh.so>
  )

#=================================================================================

# Install : Slefh
install(DIRECTORY include/
  DESTINATION ${Slefh_INCLUDE_INSTALL_DIR}
  )

install(DIRECTORY lib/
  DESTINATION ${Slefh_LIB_INSTALL_DIR}
  )

#=================================================================================

# Install export targets

install(TARGETS Slefh EXPORT Slefh_export)

install(EXPORT Slefh_export
  NAMESPACE Slefh::
  FILE SlefhTargets.cmake
  DESTINATION ${Slefh_CMAKE_INSTALL_DIR}
  )

#=================================================================================

# Install package config and version files

include(CMakePackageConfigHelpers)

# Package version file
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/SlefhConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
  )

# Package config file
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/SlefhConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/SlefhConfig.cmake
  INSTALL_DESTINATION ${Slefh_CMAKE_INSTALL_DIR}
  PATH_VARS
    Slefh_INCLUDE_INSTALL_DIR
    Slefh_LIB_INSTALL_DIR
  )

# Install the config and version files
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/SlefhConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/SlefhConfigVersion.cmake
  DESTINATION ${Slefh_CMAKE_INSTALL_DIR}
  )
