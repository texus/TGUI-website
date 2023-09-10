---
layout: page
title: Android (SDL_RENDERER backend)
breadcrumb: android sdl-renderer
---

Follow the instructions below to build SDL2, SDL2_ttf, TGUI and the example code for Android.

This guide has only been tested on Linux.


### Changing settings

The example in `TGUI/examples/android/SDL_RENDERER/` contains some hardcoded values that may need to be changed. The files to check are `app/build.gradle` and `app/jni/CMakeLists.txt`.

The `app/build.gradle` file contains the android SDK and NDK versions, the application id, the list of architectes to build and the arguments to pass to CMake.

In the `app/jni/CMakeLists.txt` file you need to provide the paths to the root directories of TGUI, SDL and SDL_ttf.


### Changing settings

Inside app/build.gradle you can change namespace, compileSdk, minSdk and targetSdk to the wanted values.

To choose the architectures to build, change the abiFilters property in app/build.gradle. The final .apk will contain all those architectures and can be installed on devices that support any of them.


### Building the example

From inside the `TGUI/examples/android/SDL_RENDERER/` folder, execute the following command to build everything. This will build any changes to SDL, SDL_ttf, TGUI and the example for all specified architectures and bundle everything into a single apk file.
```
./gradlew assembleDebug
```

If you have an emulator or device connected then you can install the apk with the following command:
```
./gradlew installDebug
```


### Troubleshooting

If the application crashes, you should run "adb logcat" and look for log output that shows the reason of the crash.

If the program exits due to an exception being thrown then you will find a line like this in the output followed by the error message:
```
"terminating with uncaught exception of type tgui::Exception:"
```
