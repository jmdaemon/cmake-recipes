# README

This CMake recipe supports multi-file CMake projects. This assumes that every subdirectory of your project also contains a CMakeLists.txt.

## Features

- Single source for project version
- Unit testing
- Color output with GCC/Clang
- Installation to /usr/local
- Out of source builds
- Support for multi project builds

## Usage

To use this project:

1. Change `myproj` to your project's name.
2. Set your project's initial `VERSION`.
3. Add a description to your project.
4. Set your project's language (C, or CXX).
5. Add the following snippet to `include/Version.h.in`:

```CMake
# include/Version.h.in
#pragma once
#ifndef VERSION_H
#define VERSION_H

#define project_VERSION_MAJOR @CMAKE_PROJECT_VERSION_MAJOR@
#define project_VERSION_MINOR @CMAKE_PROJECT_VERSION_MINOR@
#define project_VERSION_PATCH @CMAKE_PROJECT_VERSION_PATCH@

#endif
```
where project is your project's name.

Single sourcing of packages is done with the `configure_file` command.
In your project you may then import `include/Version.h` and access the project version.
