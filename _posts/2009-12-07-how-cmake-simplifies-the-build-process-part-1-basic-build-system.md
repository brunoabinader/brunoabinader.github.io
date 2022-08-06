---
title: "How CMake simplifies the build process (Part 1: Basic build system)"
date: 2009-12-07
---

 [Wikipedia](http://en.wikipedia.org/wiki/CMake) defines CMake as:

> **CMake** is a cross-platform, open-source system for managing the
build process of software using a compiler-independent method. It is
designed to support directory hierarchies and applications that depend
on multiple libraries, and for use in conjunction with native build
environments such as Make, Apple's Xcode and Microsoft Visual Studio.
It also has minimal dependencies, requiring only a C++ compiler on its
own build system.

From my experience with build systems, CMake is one of the most flexible
and easy to use ones, specially when you deal with a cross compilation
environment (actually its name stands for **C**ross Platform
**Make**). So let's get started with the guide on how to set up a
simple build system for a small project containing a few separate source
files.

CMake consists of a series of macros and functions which finds the
required components for you in a simple and precise way - meaning no
unnecessary dependencies! :smile:

> **Note:** I like the concept of shadow build directories to separate source from binary data, it gives your project a clean environment and also avoids unnecessary source modifications (specially when your source code management relies on stat to monitor source changes), among other advantages. To enforce this rule, there is a very useful CMake macro [*MacroOutOfSourceBuild.cmake*](https://github.com/esyr/cmake-hacks/blob/master/modules/MacroOutOfSourceBuild.cmake) which requires the user to build the source code outside its base directory, ensuring the developer uses a shadow build directory.

Let us start our example project. Suppose you have a small project
"Hello World" which consists of a set of files arranged like this:

```shell
$ find helloworld
helloworld/
helloworld/src
helloworld/src/main.cpp
helloworld/src/helloworld.cpp
helloworld/src/CMakeLists.txt
helloworld/src/helloworld.h
helloworld/cmake
helloworld/cmake/modules
helloworld/cmake/modules/MacroOutOfSourceBuild.cmake
helloworld/CMakeLists.txt
```

As you can see above, the source files *helloworld.h*, *helloworld.cpp*
and *main.cpp* are located inside *src/* directory, which is located on
the project's base directory. CMake states that each project directory
(including child directories) which contains source code should have a
file named *CMakeLists.txt*. This file contains the build steps for the
source files contained on that directory. The "Hello World" project
does have two directories (base directory and *src/* directory), then
two *CMakeLists.txt* files are created as follows:

## helloworld/CMakeLists.txt (base directory)

```cmake
# Project name is not mandatory, but you should use it
project(helloworld)

# States that CMake required version must be greater than 2.6
cmake_minimum_required(VERSION 2.6)

# Appends the cmake/modules path inside the MAKE_MODULE_PATH variable which stores the
# directories of additional CMake modules (ie. MacroOutOfSourceBuild.cmake):
set(CMAKE_MODULE_PATH ${helloworld_SOURCE_DIR}/cmake/modules ${CMAKE_MODULE_PATH})

# The macro below forces the build directory to be different from source directory:
include(MacroOutOfSourceBuild)

macro_ensure_out_of_source_build("${PROJECT_NAME} requires an out of source build.")

add_subdirectory(src)
```

## helloworld/src/CMakeLists.txt

```cmake
# Include the directory itself as a path to include directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Create a variable called helloworld_SOURCES containing all .cpp files:
set(helloworld_SOURCES helloworld.cpp main.cpp)

# For a large number of source files you can create it in a simpler way
# using file() function:
# file(GLOB hellworld_SOURCES *.cpp)

# Create an executable file called helloworld from sources:
add_executable(helloworld ${helloworld_SOURCES})
```

Now you create a shadow build directory (i.e. `build/`) and start
building your project, as shown with the commands below:

```shell
$ mkdir build
$ cd build
$ cmake ..
-- The C compiler identification is GNU
-- The CXX compiler identification is GNU
-- Check for working C compiler: /usr/bin/gcc
-- Check for working C compiler: /usr/bin/gcc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bruno/helloworld/build

$ make
[ 50%] Building CXX object src/CMakeFiles/helloworld.dir/helloworld.cpp.o
[100%] Building CXX object src/CMakeFiles/helloworld.dir/main.cpp.o
Linking CXX executable helloworld
[100%] Built target helloworld
```

*Voilà*! Now you have finished setting up a simple project build system.
The second part of this tutorial (Advanced build system) is cooking and
will be served soon :)
