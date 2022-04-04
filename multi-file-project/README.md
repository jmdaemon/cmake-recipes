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
5. Add the following snippet to `myproj.h.in`:

```CMake
# myproj.h.in

#define project_VERSION_MAJOR @project_VERSION_MAJOR@
#define project_VERSION_MINOR @project_VERSION_MINOR@ 
#define project_VERSION_PATCH @project_VERSION_PATCH@
```
where project is your project's name.
5. In `configure_file` change `myproj.h.in` and `myproj.h` to your project's name.

Single sourcing of packages is done with the `configure_file` command.
In your project you may then import `myproj.h` and access the project version.
