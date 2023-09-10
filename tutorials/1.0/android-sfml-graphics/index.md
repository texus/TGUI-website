---
layout: page
title: Android (SFML_GRAPHICS backend)
breadcrumb: android sfml-graphics
---

### Requirements

The tutorial assumes that the Android SDK and Android NDK are already installed correctly. SFML 2.6 or newer is highly recommended. The SFML_GRAPHICS backend was tested up to NDK r25c.

You will need to use CMake in order to build TGUI. You can download the latest version [here](https://www.cmake.org/download/).

The TGUI source code can be downloaded from the [download page](/download).

SFML should have already been installed inside the NDK before building TGUI.


### Building the library

Create a new folder inside the downloaded TGUI folder (e.g. called "build-android-arm64-v8a"). Open a terminal inside this folder and run the following command (with optionally a few changes as explained below).
```bash
cmake -S .. \
      -DTGUI_BACKEND=SFML_GRAPHICS \
      -DCMAKE_SYSTEM_NAME=Android \
      -DCMAKE_ANDROID_NDK=/path/to/ndk \
      -DCMAKE_ANDROID_ARCH_ABI=arm64-v8a \
      -DCMAKE_BUILD_TYPE=Debug
```

**CMAKE_ANDROID_NDK** has to be the path to the NDK (which will end in something like "ndk/25.2.9519653").

**CMAKE_ANDROID_ARCH_ABI** specifies the architecture to use. To use TGUI on real hardware you will need `arm64-v8a` (or `armeabi-v7a` if you still want to support 32-bit devices). To run TGUI on a simulator you will likely need `x86_64` (64-bit) or `x86` (32-bit), depending on which simulator you installed. The architecture has to match the SFML libraries.

**CMAKE_BUILD_TYPE** chooses whether you want to build a Release or Debug library.

On Windows you might also have to pass your [generator](https://cmake.org/cmake/help/v3.0/manual/cmake-generators.7.html) to the cmake command with the -G parameter. On Linux and macOS, "Unix Makefiles" will be used by default.

Depending on the generator, CMake will have created a build file or project. For "Unix Makefiles" you use the following commands in the terminal to build the library and install it to the NDK folder. The "-j2" after the make command means it uses 2 threads, if you have more cores you can increase the number to speed up the build.
```bash
make -j2
make install
```


### Running the example

To test if everything is working, you can build the example code.

If the ANDROID\_SDK\_ROOT environment variable isn't set then open the `TGUI/examples/android` folder add a "local.properties" file with the following contents, specifying the android SDK paths:
```bash
sdk.dir=/path/to/android-sdk
```

Verify that the `ndkVersion` in `app/build.gradle` inside the android example folder matches the NDK version that you are using and also check that `abiFilters` matches the `CMAKE_ANDROID_ARCH_ABI` that was passed to cmake. In the same file you can change namespace, compileSdk, minSdk and targetSdk to the wanted values.

Build the project with gradle by running the following from the command line in the `TGUI/examples/android/SFML_GRAPHICS` directory:
```bash
./gradlew assembleDebug
```

If this results in an error that SFML or TGUI is not found then double check that the `abiFilters` setting in `app/build.gradle` matches with the libraries that were build and installed earlier.

If this results in an error stating `Could not open terminal for stdout: could not get termcap entry` then set the TERM variable to dumb (e.g. run `TERM=dumb ./gradlew assembleDebug` on Linux).

If all goes well then you can now install the apk to a device (or a running simulator) by running
```bash
./gradlew installDebug
```


### Troubleshooting

If the application crashes, you should run "adb logcat" and look for log output that shows the reason of the crash.

If the program exits due to an exception being thrown then you will find a line like this in the output followed by the error message:
```
"terminating with uncaught exception of type tgui::Exception:"
```
