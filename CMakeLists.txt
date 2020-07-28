cmake_minimum_required(VERSION 3.1)

project(zvkeyboard LANGUAGES CXX C)
set(ZVKEYBOARD_VERSION_MAJOR "0")
set(ZVKEYBOARD_VERSION_MINOR "0")
set(ZVKEYBOARD_VERSION_PATCH "1")

set(PROJECT_VERSION "${ZVKEYBOARD_VERSION_MAJOR}.${ZVKEYBOARD_VERSION_MINOR}.${ZVKEYBOARD_VERSION_PATCH}")

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Options


# cmake macros
include(FeatureSummary)
include(GNUInstallDirs)

# Required Packages

# Qt5 libs
find_package(Qt5 "5.12.0" COMPONENTS Core CONFIG REQUIRED COMPONENTS)

# Compiler Flags


# All sources by type
file(GLOB LIB_HEADERS
    include/*.h
)

file(GLOB_RECURSE LIB_SOURCES
    *.cpp
)

file(GLOB LIB_RESOURCES
    *.qrc
)

# our library
add_library(ZVKeyboard SHARED
    ${LIB_SOURCES} ${LIB_HEADERS} ${LIB_RESOURCES}
)
set_target_properties(ZVKeyboard PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(ZVKeyboard PROPERTIES SOVERSION ${ZVKEYBOARD_VERSION_MAJOR})

# announce global headers - build perspective
include_directories(
    # public header
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    # generated export header
    $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
)

# announce headers - target perspective
target_include_directories(ZVKeyboard
    PUBLIC
        $<INSTALL_INTERFACE:include/zvkeyboard>
)


# linking
target_link_libraries(ZVKeyboard
    PUBLIC
    Qt5::Core
)

# install library
install(TARGETS ZVKeyboard
    EXPORT ZVKeyboardExport
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

# install public headers
install(
    DIRECTORY include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/zvkeyboard
)

# export header
include(GenerateExportHeader)
generate_export_header(ZVKeyboard)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/zvkeyboard_export.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/zvkeyboard
)

# configure *ConfigVersion.cmake
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ZVKeyboardConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY SameMinorVersion
)

# configure *Config.cmake
configure_file(ZVKeyboardConfig.cmake.in ZVKeyboardConfig.cmake @ONLY)

# install *Config(Version).cmake
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/ZVKeyboardConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/ZVKeyboardConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ZVKeyboard
)

# install targets cmake-files
install(EXPORT ZVKeyboardExport
    FILE ZVKeyboardTargets.cmake
    NAMESPACE ZeraQml::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ZVKeyboard
)


# Display feature summary
feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)











