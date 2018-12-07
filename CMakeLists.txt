cmake_minimum_required(VERSION 3.0.0)

project(nlohmann_fifo_map
  LANGUAGES CXX
  VERSION "0.0.0-0dfbf5d")

include_directories(
    src
)

add_library(fifo_map INTERFACE)

### Install (https://github.com/forexample/package-example) ###
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${version_config}" COMPATIBILITY AnyNewerVersion
)

# Note: use 'targets_export_name'
configure_package_config_file(
    "cmake/Config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION "${config_install_dir}"
)

install(
    TARGETS fifo_map
    EXPORT "${targets_export_name}"
    INCLUDES DESTINATION "${include_install_dir}"
)

install(
    DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/src/
    DESTINATION include/nlohmann
    FILES_MATCHING PATTERN "*.hpp"
)

install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

install(
    EXPORT "${targets_export_name}"
    NAMESPACE "${namespace}"
    DESTINATION "${config_install_dir}"
)
