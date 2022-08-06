---
title: "How CMake simplifies the build process: Part 2 (Advanced build system)"
date: 2009-12-09
---

My [previous post](/2009/12/07/how-cmake-simplifies-the-build-process-part-1-basic-build-system/) explained how [CMake](http://www.cmake.org/) could manage a simple project (containing only one definition and implementation file).  Now we can have a look at a complex project, containing a common shared library, examples and unit tests. In this case, instead of a dumb HelloWorld project, I’ve chosen [Itemviews-NG](http://labs.trolltech.com/page/Projects/Itemview/ItemviewsNG), an experimental project hosted by the Qt Labs and currently uses `qmake` as build system. The project itself consists of the following:

* A common shared library containing the base classes
* A collection of examples for each class from the library
* A collection of unit tests for each class from the library

> **NOTE**: Documentation won’t be created for now – Itemview-NG uses `qdoc` to generate documentation, which AFAIK isn’t well integrated with CMake yet. There will be another post about using `doxygen` with CMake, together with `lcov` coverage and `gcov` HTML output integrated.

As a starting point, the project itself contains various files which needs to be processed by the [MOC](http://qt.nokia.com/doc/4.6/moc.html). To simplify this process, CMake can use the `automoc` tool which automatically finds the required files to be preprocessed and includes them at the target build. Qt libraries and includes are also provided by the Qt4 CMake package (built-in).

To use the Qt4+ automoc cmake packages, you can include the following code on the `CMakeFiles.txt` located at the project’s root directory:

```cmake
# Qt4 package is mandatory
find_package(Qt4 REQUIRED)
include(${QT_USE_FILE})
 
# Automoc4 is also mandatory
find_package(Automoc4 REQUIRED)
```

Now let’s see how the CMakeFiles.txt from the `src/` directory changes:

```cmake
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Create a variable containing a list of all implementation files
file(GLOB itemviews-ng_SOURCES *.cpp)
 
# The same applies for headers which generates MOC files (excluding private implementation ones)
file(GLOB itemviews-ng HEADERS *[^_p].h)
 
# Now the magic happens: The function below is responsible for generating the MOC files)
automoc4_moc_headers(itemviews-ng ${itemviews-ng_HEADERS})
 
# Creates a target itemviews-ng which creates a shard library with the given sources
automoc4_add_library(itemviews-ng SHARED ${itemviews-ng_SOURCES})
 
# Tells the shared library to be linked against Qt ones
target_link_libraries(itemviews-ng ${QT_LIBRARIES})
 
# Installs the header files into the {build_dir}/include/itemviews-ng directory
install(FILES ${itemviews-ng_HEADERS} DESTINATION include/itemviews-ng)
 
# Installs the target file (libitemviews-ng.so) into the {build_dir}/lib directory
install(TARGETS itemviews-ng LIBRARY DESTINATION lib)
```

Now every example or test from the project which uses that library could use the target `itemviews-ng` as a link dependency. As each examples and tests are compiled in a similar way, we can use functions which eases the compilation process. Below is an example of those functions:

```cmake
function(example example_NAME example_SOURCES example_HEADERS)
automoc4_moc_headers(${example_NAME}_example ${examples_HEADERS})
# The third argument (resource files) is optional:
set(example_RESOURCES ${ARGV3})
if(example_RESOURCES)
    automoc4_add_executable(${example_NAME}_example
        ${example_SOURCES} {example_RESOURCES})
else(example_RESOURCES)
    automoc4_add_executable(${example_NAME}_example
        ${example_SOURCES})    endif(example_RESOURCES)
    target_link_libraries(${example_NAME}_example itemviews-ng
       {QT_LIBRARIES})
endfunction(example)
 
function(test test_NAME test_SOURCES)
set(test_RESOURCES ${ARGV2})
if(test_RESOURCES)
    automoc4_add_executable(${test_NAME}_test
        {test_SOURCES} ${test_RESOURCES})
else(test_RESOURCES)
    automoc4_add_executable(${test_NAME}_test
        ${test_SOURCES})    endif(test_RESOURCES)
    target_link_libraries(${test_NAME}_test itemviews-ng
        ${QT_LIBRARIES})
include_directories(${itemviews-ng_BINARY_DIR}/tests/${test_NAME})
add_test(${test_NAME} ${test_NAME}_test)
add_dependencies(check ${test_NAME}_test)
endfunction(test)
```

The functions above can be inserted into a `.cmake` file (eg: `InternalMacros.cmake`) that goes in the `cmake/modules` directory. To load it, put the following line inside `CMakeLists.txt` from the root directory:

```cmake
include(InternalMacros)
```

Now you can create a `CMakeLists.txt` for each example like this:

```cmake
set(mycustomexample_SOURCES mycustomexample.cpp main.cpp )
set(mycustomexample_HEADERS mycustomexample.h )
 
example(mycustomexample "${mycustomexample_SOURCES}" "${mycustomexample_HEADERS}")
```

The same applies for tests:

```cmake
test(mycustomtest "${mycustomtest_SOURCES}")
```

I’ve made a branch [cmake](http://qt.gitorious.org/qt-labs/brunoabinader-itemviews-ng) at my personal gitorious [itemviews-ng clone](http://qt.gitorious.org/%7Ebrunoabinader/qt-labs/brunoabinader-itemviews-ng). You can have a look at it to see how does the code explained above behaves.