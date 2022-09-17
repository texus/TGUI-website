---
layout: page
title: iOS (SFML_GRAPHICS backend)
breadcrumb: ios sfml-graphics
---

### Requirements

At least SFML 2.5 is required.

You must have already build SFML for iOS and verified that you can run your sfml app on your device or emulator. Only then should you proceed with installing TGUI.

You will need to use CMake in order to build TGUI. You can download the latest version [here](https://cmake.org/download/). After installing the GUI app, you will need to run the following from a terminal to make cmake usable from the terminal:
```
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install
```

The TGUI source code can be downloaded from the [download page](/download).

TGUI currently only supports building a static library for iOS, there is no option to build a dynamic library or framework.

### CMake

For information about cross-compiling options for iOS with CMake that aren't specific to TGUI, see [CMake's toolchains manual](https://cmake.org/cmake/help/latest/manual/cmake-toolchains.7.html#cross-compiling-for-ios-tvos-or-watchos).

Execute something similar to the following in a terminal from within the TGUI folder:
```
cmake -S . -B build-ios
  -GXcode \
  -DCMAKE_SYSTEM_NAME=iOS \
  -DCMAKE_OSX_ARCHITECTURES=arm64 \
  -DCMAKE_OSX_DEPLOYMENT_TARGET=13.0 \
  -DCMAKE_INSTALL_PREFIX=`pwd`/install-ios \
  -DTGUI_BACKEND=SFML_GRAPHICS
  -DFreeType_LIB=/Path/To/SFML/extlibs/libs-ios/libfreetype.a
```

Here is an explanation of each option:  
- `-S .`: Specifies that the location of the source files is the current working directory.
- `-B build-ios`: Specifies that will create a new folder called "build-ios" in which we will store temperory files.
- `-GXcode`: Specifies that we will create an Xcode project for building the iOS library.
- `-DCMAKE_OSX_ARCHITECTURES=arm64`: Specifies a list of cpu architectures for which to build the library, seperated by semicolons if multiple.
- `-DCMAKE_OSX_DEPLOYMENT_TARGET=13.0`: Specifies the minimum iOS version to build for.
- ``-DCMAKE_INSTALL_PREFIX=`pwd`/install-ios``: Specifies that the library will be installed in a new "install-ios" folder in the current working directory.
- `-DTGUI_BACKEND=SFML_GRAPHICS`: Specifies that TGUI should be build with the SFML_GRAPHICS backend
- `-DFreeType_LIB=/Path/To/SFML/extlibs/libs-ios/libfreetype.a`: SFML might find libraries for macOS instead of iOS when searching for its dependencies. This is most likely going to happen for FreeType. So tell SFML which library it should use.

If SFML can't be found, you may need to add the `-DSFML_DIR=...` property with a directory that contains the SFMLConfig.cmake file (the path usually ends with `lib/cmake/SFML/`).

### Building the library

The previous step will have created an Xcode project for building TGUI (if `-GXcode` was used). There are some issues/limitations when opening the project directly with Xcode, so it is best to build it with the following command:
```
cmake --build build-ios --config Release --target install
```

If all goes well, the newly created install-ios folder will contain a lib folder with the static tgui library in it (inside a Debug or Release subfolder if you used Xcode).

### Using TGUI

<p><span class="Red">Note that using TGUI in an iOS app has never actually been tested.</span></p>

You should be able to just drag the tgui-s.a (or tgui-s-d.a) file into your project.