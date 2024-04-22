---
layout: page
title: iOS (SDL_TTF_GLES2 backend)
breadcrumb: ios sdl-renderer
---

### Requirements

You must have already build SDL2 and SDL2_ttf for iOS and verified that you can run your SDL2 app on your device or emulator. Only then should you proceed with installing TGUI.

You will need to use CMake in order to build TGUI. You can download the latest version [here](https://cmake.org/download/). After installing the GUI app, you will need to run the following from a terminal to make cmake usable from the terminal:
```
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install
```

The TGUI source code can be downloaded from the [download page](/download).

### CMake

For information about cross-compiling options for iOS with CMake that aren't specific to TGUI, see [CMake's toolchains manual](https://cmake.org/cmake/help/latest/manual/cmake-toolchains.7.html#cross-compiling-for-ios-tvos-or-watchos).

Execute something similar to the following in a terminal from within the TGUI folder:
```
cmake -S . -B build-ios \
  -GXcode \
  -DCMAKE_SYSTEM_NAME=iOS \
  -DCMAKE_OSX_ARCHITECTURES=arm64 \
  -DCMAKE_OSX_DEPLOYMENT_TARGET=13.0 \
  -DCMAKE_INSTALL_PREFIX=`pwd`/install-ios \
  -DBUILD_SHARED_LIBS=FALSE \
  -DTGUI_SKIP_SDL_CONFIG=TRUE \
  -DTGUI_BACKEND=SDL_TTF_GLES2 \
  -DSDL2_LIBRARY=/Path/to/libSDL2.a \
  -DSDL2_INCLUDE_DIR=/Path/to/SDL/include/ \
  -DSDL2_TTF_LIBRARY=/Path/to/libSDL2_ttf.a \
  -DSDL2_TTF_INCLUDE_DIR=/Path/to/SDL_ttf/
```

Here is an explanation of each option:  
- `-S .`: Specifies that the location of the source files is the current working directory.
- `-B build-ios`: Specifies that will create a new folder called "build-ios" in which we will store temperory files.
- `-GXcode`: Specifies that we will create an Xcode project for building the iOS library.
- `-DCMAKE_OSX_ARCHITECTURES=arm64`: Specifies a list of cpu architectures for which to build the library, seperated by semicolons if multiple.
- `-DCMAKE_OSX_DEPLOYMENT_TARGET=13.0`: Specifies the minimum iOS version to build for.
- ``-DCMAKE_INSTALL_PREFIX=`pwd`/install-ios``: Specifies that the library will be installed in a new "install-ios" folder in the current working directory.
- `-DBUILD_SHARED_LIBS=FALSE`: Build a static library (file with `.a` extension)
- `-DTGUI_SKIP_SDL_CONFIG=TRUE`: Prevents finding a macOS config file which would be used instead of the specified iOS library
- `-DTGUI_BACKEND=SDL_TTF_GLES2`: Specifies that TGUI should be build with the SDL\_TTF\_GLES2 backend
- `-DSDL2_LIBRARY=libSDL2.a`: Specifies the iOS SDL library (path and filename)
- `-DSDL2_INCLUDE_DIR=/Path/to/SDL/include/`: Specifies the location of the SDL include files (directory containing SDL.h and other SDL headers)
- `-DSDL2_TTF_LIBRARY=libSDL2_ttf.a`: Specifies the iOS SDL_ttf library (path and filename)
- `-DSDL2_TTF_INCLUDE_DIR=/Path/to/SDL_ttf/`: Path to the SDL_ttf header (directory containing SDL_ttf.h)

### Building the library

The previous step will have created an Xcode project for building TGUI (if `-GXcode` was used). You can build it from the terminal with the following command:
```
cmake --build build-ios --config Release --target install
```

If all goes well, the newly created install-ios folder will contain a lib folder with the static tgui library in it (inside a Debug or Release subfolder if you used Xcode).

### Using TGUI

<p><span class="Red">Note that using TGUI in an iOS app has never actually been tested.</span></p>

You should be able to just drag the tgui-s.a (or tgui-s-d.a) file into your project.
