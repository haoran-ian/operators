cmake_minimum_required(VERSION 3.17)

file(READ VERSION VERSION_STRING)
string(STRIP ${VERSION_STRING} ${VERSION_STRING})
string(REGEX REPLACE "\n$" "" VERSION_STRING "${VERSION_STRING}")
project(operators    
    VERSION "${VERSION_STRING}"
    LANGUAGES CXX  
)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)


add_library(operators INTERFACE)
target_include_directories(operators INTERFACE
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
target_compile_features(operators INTERFACE cxx_std_17)

# Generate package config files.
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/cmake/operators-config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/operators-config.cmake
INSTALL_DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/operators
) 
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/operators-config-version.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)


# Define installation rules.
include(GNUInstallDirs)

install(TARGETS operators
    EXPORT operators-targets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
FILES
    ${CMAKE_CURRENT_BINARY_DIR}/operators-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/operators-config-version.cmake
DESTINATION
    ${CMAKE_INSTALL_LIBDIR}/cmake/operators
)

install(EXPORT operators-targets
    FILE operators-targets.cmake
    NAMESPACE operators::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/operators
)
