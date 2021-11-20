---
layout: page
title: Selecting a backend in CMake
breadcrumb: selecting backend in cmake
---

TGUI is not a standalone library, it depends on libraries such as SFML, SDL or GLFW, which you will likely need to use in your own code as well. The choice of the backend should thus be based on which libraries you want to use in your own program.

When building the library with CMake, the TGUI\_BACKEND property has to be set to the wanted backend. It is also possible to build the library with multiple backends (or with none if you want to provide one yourself) by setting TGUI\_BACKEND to "Custom" and setting the boolean TGUI\_HAS\_BACKEND\_XXX values.

The following backends exist and can be given as value to TGUI\_BACKEND:
<table class="with-borders">
  <thead>
    <tr>
      <th>Backend</th>
      <th>Platforms</th>
      <th>Dependencies</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>SFML_GRAPHICS</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Android.svg" title="Android"/></div>
        <div class="platform-icon">
          <picture class="dark-compatible">
            <source srcset="/resources/PlatformIcons/iOS-white.svg" media="(prefers-color-scheme: dark)">
            <img src="/resources/PlatformIcons/iOS.svg" title="iOS"/>
          </picture>
        </div>
      </td>
      <td>sfml-graphics <span class="BackendDependencyVersion">(>=&nbsp;2.5)</span></td>
    </tr>
    <tr>
      <td><strong>SFML_OPENGL3</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>sfml-window <span class="BackendDependencyVersion">(>=&nbsp;2.5) +</span> FreeType <span class="BackendDependencyVersion">(>=&nbsp;2.6) +</span> OpenGL <span class="BackendDependencyVersion">(>=&nbsp;3.3)</span></td>
    </tr>
    <tr>
      <td><strong>SDL_RENDERER</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Android.svg" title="Android"/></div>
        <div class="platform-icon">
          <picture class="dark-compatible">
            <source srcset="/resources/PlatformIcons/iOS-white.svg" media="(prefers-color-scheme: dark)">
            <img src="/resources/PlatformIcons/iOS.svg" title="iOS"/>
          </picture>
        </div>
      </td>
      <td>SDL2 <span class="BackendDependencyVersion">(>=&nbsp;2.0.18) +</span> SDL2_ttf <span class="BackendDependencyVersion">(>=&nbsp;2.0.14)</span></td>
    </tr>
    <tr>
      <td><strong>SDL_OPENGL3</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>SDL2 <span class="BackendDependencyVersion">(>=&nbsp;2.0.6) +</span> FreeType <span class="BackendDependencyVersion">(>=&nbsp;2.6) +</span> OpenGL <span class="BackendDependencyVersion">(>=&nbsp;3.3)</span></td>
    </tr>
    <tr>
      <td><strong>SDL_GLES2</strong></td>
      <td>
        <div class="platform-icon"></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>SDL2 <span class="BackendDependencyVersion">(>=&nbsp;2.0.6) +</span> FreeType <span class="BackendDependencyVersion">(>=&nbsp;2.6) +</span> OpenGL ES <span class="BackendDependencyVersion">(>=&nbsp;2.0)</span></td>
    </tr>
    <tr>
      <td><strong>SDL_TTF_OPENGL3</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>SDL2 <span class="BackendDependencyVersion">(>=&nbsp;2.0.6) +</span> SDL2_ttf <span class="BackendDependencyVersion">(>=&nbsp;2.0.14) +</span> OpenGL <span class="BackendDependencyVersion">(>=&nbsp;3.3)</span></td>
    </tr>
    <tr>
      <td><strong>SDL_TTF_GLES2</strong></td>
      <td>
        <div class="platform-icon"></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Android.svg" title="Android"/></div>
        <div class="platform-icon"></div>
      </td>
      <td>SDL2 <span class="BackendDependencyVersion">(>=&nbsp;2.0.6) +</span> SDL2_ttf <span class="BackendDependencyVersion">(>=&nbsp;2.0.14) +</span> OpenGL ES <span class="BackendDependencyVersion">(>=&nbsp;2.0)</span></td>
    </tr>
    <tr>
      <td><strong>GLFW_OPENGL3</strong></td>
      <td>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Windows.svg" title="Windows"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/macOS.svg" title="macOS" class="dark-compatible"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>GLFW <span class="BackendDependencyVersion">(>=&nbsp;3.2) +</span> FreeType <span class="BackendDependencyVersion">(>=&nbsp;2.6) +</span> OpenGL <span class="BackendDependencyVersion">(>=&nbsp;3.3)</span></td>
    </tr>
    <tr>
      <td><strong>GLFW_GLES2</strong></td>
      <td>
        <div class="platform-icon"></div>
        <div class="platform-icon"><img src="/resources/PlatformIcons/Linux.svg" title="Linux"/></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
        <div class="platform-icon"></div>
      </td>
      <td>GLFW <span class="BackendDependencyVersion">(>=&nbsp;3.2) +</span> FreeType <span class="BackendDependencyVersion">(>=&nbsp;2.6) +</span> OpenGL ES <span class="BackendDependencyVersion">(>=&nbsp;2.0)</span></td>
    </tr>
  </tbody>
</table>

Platform remarks:

- **Raspberry Pi**: Should be supported by backends that support Linux, but GLES backends are recommended over OpenGL backends.

- **BSD**: Should be supported by backends that support Linux, if the dependencies also support BSD.

- **macOS**: TGUI is known to work, but guides may be incomplete due to lack of experience with macOS.

- **iOS**: Building TGUI was tested, but running a program that uses it (either in simulator or on real device) has not been tested!


### Dependencies

Once the backend is chosen, CMake will try to find the dependencies. On some platforms (e.g. Linux), CMake will be able to automatically find them if they are installed, but on other platforms (e.g. Windows) you will have to manually tell CMake where to find the dependencies.

Below you find some information about which properties you need to set when CMake can't find the dependencies automatically.

#### SFML

To find SFML, the SFML\_DIR variable needs to be set to the directory that contains the SFMLConfig.cmake file (just the path without the filename). If you built SFML yourself without installing it then this will be your SFML build directory, otherwise the file can be found in the `lib/cmake/SFML` subfolder of your installed or downloaded SFML directory.

Note that the the file you need is called exactly "SFMLConfig.cmake", the "SFMLConfig.cmake.in" file is not the correct one.

Usually you will use static SFML libraries when linking TGUI statically, but if for some reason you want to link to SFML dynamically while TGUI\_SHARED\_LIBS=FALSE then you can set SFML\_STATIC\_LIBRARIES to FALSE.

#### SDL

TGUI will first attempt to find an SDL2Config.cmake file. If CMake can't find it and you have such file (usually in a lib/cmake/SDL2 subfolder) then you can set the SDL2\_DIR property to the path containing this file.

If you downloaded the Development Libraries from the [SDL download page](https://libsdl.org/download-2.0.php) then you won't have the SDL2Config.cmake file. In this case you can set the SDL2\_PATH property to the root of the downloaded SDL directory.

Note that you should not confuse SDL2\_DIR and SDL2\_PATH. The first one is a directory that contains SDL2Config.cmake, the latter is a directory that contains `include` and `lib` subdirectories (or just an `SDL2.framework` file on macOS). Only one of the two has to be provided.

#### SDL_ttf

If SDL\_ttf isn't automatically found then set the SDL2\_TTF\_PATH variable to the root folder of the Development Libraries that you downloaded from the [SDL\_ttf download page](https://www.libsdl.org/projects/SDL_ttf/).

The value of SDL2\_TTF\_PATH should be a directory that either contains `include` and `lib` subdirectories, or a directory containing `SDL2_ttf.framework` (for macOS).

#### GLFW

TGUI will first attempt to find a glfw3Config.cmake file. If CMake can't find it and you have such file (usually in a lib/cmake/glfw3 subfolder) then you can set the glfw3\_DIR property to the path containing this file.

Alternatively, you can manually specify the values for GLFW\_INCLUDE\_DIR and GLFW\_LIBRARY. Note that GLFW\_INCLUDE\_DIR has to be the path that contains the GLFW subdirectory, not the path that contains glfw3.h file (which has to be located inside the GLFW subdirectory).

#### FreeType

CMake provides 3 properties that have to be set in order to find FreeType:
- `FREETYPE_INCLUDE_DIR_ft2build`: The directory that contains the ft2build.h header file
- `FREETYPE_INCLUDE_DIR_freetype2`: The directory that contains the freetype header files and the config subdirectory
- `FREETYPE_LIBRARIES`: The freetype library file to link against

If you downloaded the Windows libraries from [github.com/ubawurinna/freetype-windows-binaries](https://github.com/ubawurinna/freetype-windows-binaries) then you can set FREETYPE\_WINDOWS\_BINARIES\_PATH to the root directory to automatically select the correct include and library files. Otherwise you can ignore this property and set the 3 properties listed above.

Warning: due to a bug in FreeType, version 2.11.0 can NOT be used on Windows.
