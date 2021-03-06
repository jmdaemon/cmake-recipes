cmake_minimum_required(VERSION 3.12)

# Set project settings
project(
    myproj
    VERSION 0.1.0
    DESCRIPTION "" 
    LANGUAGES CXX
    )

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME) 
    # Set standard
    set(CMAKE_CXX_STANDARD 20) 
    set(CMAKE_CXX_STANDARD_REQUIRED ON) 

    # Useful for CCLS and other C/C++ language servers
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
    set(CMAKE_CXX_EXTENSIONS OFF) 
    set_property(GLOBAL PROPERTY USE_FOLDERS ON) 
    # Include unit tests
    include (CTest)
endif()

### Require out-of-source builds
file(TO_CMAKE_PATH "${PROJECT_BINARY_DIR}/CMakeLists.txt" LOC_PATH)
if (EXISTS "${LOC_PATH}")
    message(FATAL_ERROR "You cannot build in a source directory (or any directory with "
                        "CMakeLists.txt file). Please make a build subdirectory. Feel free to "
                        "remove CMakeCache.txt and CMakeFiles.")
endif()

# Include CMake modules
set( CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/CMake/modules/" "${PROJECT_SOURCE_DIR}/cmake" )

# Single source project version
configure_file (
  "${PROJECT_SOURCE_DIR}/include/Version.h.in"
  "${PROJECT_BINARY_DIR}/include/Version.h"
)

# Include dependencies
include(FetchContent)

# Include doctest as a dependency if building unit tests
if (BUILD_TESTING)
include(ExternalProject)
  find_package(Git REQUIRED)
  ExternalProject_Add( # https://github.com/onqtam/doctest/blob/master/doc/markdown/build-systems.md
      doctest
      PREFIX ${CMAKE_BINARY_DIR}/doctest
      GIT_REPOSITORY https://github.com/onqtam/doctest.git
      TIMEOUT 10
      UPDATE_COMMAND ${GIT_EXECUTABLE} pull
      CONFIGURE_COMMAND ""
      BUILD_COMMAND ""
      INSTALL_COMMAND ""
      LOG_DOWNLOAD ON
  )
  
  ExternalProject_Get_Property(doctest source_dir) # Expose required variable (DOCTEST_INCLUDE_DIR) to parent scope
  set(DOCTEST_INCLUDE_DIR ${source_dir}/doctest CACHE INTERNAL "Path to include folder for doctest")
endif()

# Use color in GCC/Clang errors
set(CMAKE_CXX_FLAGS -fdiagnostics-color=always)
set(CMAKE_C_FLAGS -fdiagnostics-color=always)

# Add our project source directories
add_subdirectory(src)
add_subdirectory(app)

# Build unit tests
if(BUILD_TESTING)
  add_subdirectory(test)
endif()

# Install to /usr/local/
set(CMAKE_INSTALL_PREFIX /usr/local)

# Set Release and Debug directories
install(TARGETS ${OGGEX} DESTINATION bin)
install(TARGETS ${OGGEX} CONFIGURATIONS Debug   RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/Debug/bin)
install(TARGETS ${OGGEX} CONFIGURATIONS Release RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/Release/bin)

# Define Ninja uninstall
add_custom_target(uninstall COMMAND xargs rm < ${CMAKE_CURRENT_BINARY_DIR}/install_manifest.txt)
