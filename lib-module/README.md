# README

This CMakeLists.txt snippet will build a library target for your project.

You can use this recipe in a multi-file/module project.

## Usage

This CMake recipe assumes you have your headers in a top level `include` directory.

1. Include your library's headers in `${LIBRARY_HEADERS}/`
2. Change the target `mylib` to the target name of your library.
3. Change `MyLib.cpp` to your library's sources in `target_sources()`.
4. Compile normally in your project.

Note that since this recipe assumes you are doing a multi file build, this CMakeLists.txt should be placed into a subdirectory of your main project and added with:
``` CMake
add_subdirectory("mylib")
```
where `mylib` is the name of your library.

Also note that if you don't want to use a top level `include` directory, you can specify the current project directory instead, or otherwise ignore it.
