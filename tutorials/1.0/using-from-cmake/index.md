---
layout: page
title: Using TGUI from a CMake project
breadcrumb: using from cmake
---

### Approach 1: find_package

If you are using prebuilt TGUI libraries, or if you have already built TGUI with CMake then you can let your project search for the existing files using `find_package`.

The two things that need to be added to your cmake script are finding TGUI and linking your project to it. To do this, simply call `find_package` with the required TGUI version and then call `target_link_libraries` to link with the `TGUI::TGUI` target. The `target_link_libraries` call will not only add the library to the linker, but also add the correct include directories to the project so that the TGUI header files can be found.
```cmake
find_package(TGUI 1 REQUIRED)
target_link_libraries(DemoProject PRIVATE TGUI::TGUI)
```

If you have both static and dynamic TGUI libraries installed then you should set the `TGUI_STATIC_LIBRARIES` boolean property to select which library to link to. If you e.g. want to link statically then you would need to put the following line BEFORE calling find_package:
```cmake
set(TGUI_STATIC_LIBRARIES TRUE)
```

#### TGUI not found

If you pass `REQUIRED` to `find_package` then it will halt the cmake script and show an error when TGUI isn't found. Otherwise you have to manually check whether `TGUI_FOUND` was set to TRUE or not.

When TGUI is installed, there should be a TGUIConfig.cmake file in the `lib/cmake/TGUI` subdirectory. If you built TGUI yourself (without installing it) then TGUIConfig.cmake should exist inside your build folder. In order to make CMake find TGUI, simply set the `TGUI_DIR` property (which is created when `find_package` fails) to the path in which the TGUIConfig.cmake can be found.

Note that CMake can also complain about not finding TGUI when it fails to find one of TGUI's dependencies. The exact error message will be different in this case. The [backends tutorial](../backends/) contain information about the properties that you need to set to help CMake find the dependencies when it can't find them automatically.


### Approach 2: FetchContent

If you want to download and build TGUI together with your project instead of first building TGUI separately, then you can use CMake's `FetchContent` module.

Before calling `FetchContent_MakeAvailable`, you should set some properties that choose how TGUI is build. These are the same properties that are explained at the top of the [Building TGUI from source code with CMake](../cmake/) tutorial. The `target_link_libraries` call will not only add the library to the linker, but also add the correct include directories to the project so that the TGUI header files can be found.
```cmake
include(FetchContent)

# Set properties that will be used while building TGUI
set(BUILD_SHARED_LIBS OFF)      # Determines whether TGUI build a static or dynamic/shared library
set(TGUI_BACKEND SDL_RENDERER)  # Sets which backend TGUI will use

FetchContent_Declare(
  TGUI
  GIT_REPOSITORY https://github.com/texus/TGUI
  GIT_TAG v1.4.0  # Change this to wanted version
)
FetchContent_MakeAvailable(TGUI)

target_link_libraries(DemoProject PRIVATE TGUI::TGUI)
```
