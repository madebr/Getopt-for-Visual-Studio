cmake_minimum_required(VERSION 3.1)

if(DEFINED PROJECT_NAME)
  set(GETOPT_SUBPROJECT ON)
endif()

project(getopt_for_visual_studio LANGUAGES C)

if(NOT "${CMAKE_C_COMPILER_ID}" STREQUAL "MSVC")
    message(FATAL_ERROR "${PROJECT_NAME} is only available on MSVC")
endif()

include(CMakePackageConfigHelpers)
include(GNUInstallDirs)

add_library(getopt INTERFACE)
add_library(getopt::getopt ALIAS getopt)
target_include_directories(getopt INTERFACE
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>"
    "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)

option(GETOPT_TESTING "Enable tests for getopt")
if(GETOPT_TESTING)
    enable_testing()
    add_subdirectory(test)
endif()


if(NOT GETOPT_SUBPROJECT)
    configure_package_config_file(getopt-config.cmake.in getopt-config.cmake
        INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/getopt")

    install(TARGETS getopt EXPORT getopt_exports)
    install(FILES getopt.h DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")
    install(FILES "${CMAKE_CURRENT_BINARY_DIR}/getopt-config.cmake" DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/getopt")
    install(EXPORT getopt_exports DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/getopt"
        NAMESPACE getopt:: FILE "getopt.cmake")
endif()
