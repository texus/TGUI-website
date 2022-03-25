---
layout: page
title: Using TGUI from a CMake project
breadcrumb: using from cmake
---

The two things that need to be added to your cmake script are finding TGUI and linking your project to it. To do this, simply call `find_package` with the required TGUI version and then call `target_link_libraries` to link with the `TGUI::TGUI` target. The `target_link_libraries` call will not only add the library to the linker, but also add the correct include directories to the project so that the TGUI header files can be found.
```c++
find_package(TGUI 0.10 REQUIRED)
target_link_libraries(DemoProject PRIVATE TGUI::TGUI)
```

If you have both static and dynamic TGUI libraries installed then you should set the `TGUI_STATIC_LIBRARIES` boolean property to select which library to link to. If you e.g. want to link statically then you would need to put the following line BEFORE calling find_package:
```c++
set(TGUI_STATIC_LIBRARIES TRUE)
```

### TGUI not found

If you pass `REQUIRED` to `find_package` then it will halt the cmake script and show an error when TGUI isn't found. Otherwise you have to manually check whether `TGUI_FOUND` was set to TRUE or not.

When TGUI is installed, there should be a TGUIConfig.cmake file in the `lib/cmake/TGUI` subdirectory. If you built TGUI yourself (without installing it) then TGUIConfig.cmake should exist inside your build folder. In order to make CMake find TGUI, simply set the `TGUI_DIR` property (which is created when `find_package` fails) to the path in which the TGUIConfig.cmake can be found.

Note that CMake can also complain about not finding TGUI when it fails to find one of TGUI's dependencies. The exact error message will be different in this case. The [backends tutorial](../backends/) contain information about the properties that you need to set to help CMake find the dependencies when it can't find them automatically.
