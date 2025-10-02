cmake_minimum_required(VERSION 3.23)

project(stb VERSION 1.0.0 LANGUAGES C)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

add_library(stb INTERFACE)
add_library(stb::stb ALIAS stb)

target_include_directories(stb INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_compile_options(stb INTERFACE
  -Wall -Wextra -Wpedantic
)

target_compile_features(stb INTERFACE c_std_17)

target_sources(stb INTERFACE
  FILE_SET HEADERS
  BASE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}
  FILES
    ${CMAKE_CURRENT_SOURCE_DIR}/stb/stb_image.h
    ${CMAKE_CURRENT_SOURCE_DIR}/stb/stb_image_write.h
)

# --------------- Export for the build tree  ----------------

export(
  EXPORT ${PROJECT_NAME}Targets
  FILE "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Targets.cmake"
  NAMESPACE ${PROJECT_NAME}::
)

# -------------------- Install & package --------------------

install(TARGETS stb
  EXPORT stbTargets
  FILE_SET HEADERS
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/stbConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
  INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/stb"
)

install(EXPORT stbTargets
  NAMESPACE stb::
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/stb"
)

install(FILES
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfig.cmake"
  "${CMAKE_CURRENT_BINARY_DIR}/stbConfigVersion.cmake"
  DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/stb"
)
