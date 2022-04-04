# README

This repository contains some common CMake recipes that I've found useful for some of my projects.

## Creating Debian Packages

To create packages for Debian, include:

``` CMake
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Your Name") # required
set(CPACK_PACKAGE_DIRECTORY "packages/${CMAKE_PROJECT_NAME}-${CMAKE_PROJECT_VERSION}")
#set(CPACK_DEBIAN_PACKAGE_DEPENDS "Your project dependencies")
set(CPACK_PACKAGE_DESCRIPTION_FILE "${CMAKE_CURRENT_SOURCE_DIR}/README.md")
set(CPACK_GENERATOR "DEB;TGZ;ZIP") # Create .deb, .tgz, .zip files
include(CPack)
```

## Creating Debug/Release Versions

To create debug/release versions in:

- build/debug
- build/release

Add the following to your CMakeLists.txt file:
```
install(TARGETS ${OGGEX} DESTINATION bin)
install(TARGETS ${OGGEX} CONFIGURATIONS Debug   RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin/debug)
install(TARGETS ${OGGEX} CONFIGURATIONS Release RUNTIME DESTINATION ${PROJECT_BINARY_DIR}/bin/release)
```
