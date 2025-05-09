---
layout: page
title: Download
redirect_from: /bindings/index.html
---

### TGUI 1.x-dev

**In development version** from [github](https://github.com/texus/TGUI/). Minimum supported compilers: GCC 7, Clang 6, VS2019

<p>
  {% include button.ext text="Source code" link="https://github.com/texus/TGUI/archive/1.x.zip" style="Orange" %}<br><br>
  {% include button.ext text="Visual Studio - SFML_GRAPHICS - 32bit" link="https://github.com/texus/TGUI/releases/download/nightly_build/TGUI-1.x-nightly-VisualStudio-32bit-for-SFML-3.0.0.zip" style="Orange" %}
  {% include button.ext text="Visual Studio - SFML_GRAPHICS - 64bit" link="https://github.com/texus/TGUI/releases/download/nightly_build/TGUI-1.x-nightly-VisualStudio-64bit-for-SFML-3.0.0.zip" style="Orange" %} (use with <a href="https://www.sfml-dev.org/download/sfml/3.0.0/">SFML 3.0.0</a> binaries)<br>
</p>

Packages:  
**AUR** (Arch Linux): [tgui-git](https://aur.archlinux.org/packages/tgui-git/) (official, contains all backends)  

### TGUI 1.9

**Latest stable version**. Minimum supported compilers: GCC 7, Clang 6, VS2019

{% include button.ext text="Source code" link="https://github.com/texus/TGUI/archive/v1.9.0.zip" style="Green" %}

Precompiled libraries for Windows with SFML_GRAPHICS backend: (requires matching libraries from [SFML 3.0.0](https://www.sfml-dev.org/download/sfml/3.0.0/))

<table>
<tr>
  <td><p>{% include button.ext text="Visual C++ 17 (2022) - 32bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-vc17-32bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
  <td><p>{% include button.ext text="Visual C++ 17 (2022) - 64bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-vc17-64bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
</tr>
<tr>
  <td><p>{% include button.ext text="Visual C++ 16 (2019) - 32bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-vc16-32bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
  <td><p>{% include button.ext text="Visual C++ 16 (2019) - 64bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-vc16-64bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
</tr>
<tr>
  <td><p>{% include button.ext text="GCC 14.2.0 MinGW (UCRT) (POSIX) - 32bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-mingw-14.2.0-32bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
  <td><p>{% include button.ext text="GCC 14.2.0 MinGW (UCRT) (POSIX) - 64bit" link="https://github.com/texus/TGUI/releases/download/v1.9.0/TGUI-1.9.0-mingw-14.2.0-64bit-for-SFML-3.0.0.zip" style="Green DownloadBtnInTable" %}</p></td>
</tr>
</table>

Packages:  
**vcpkg** (Windows, Linux, macOS): [tgui](https://github.com/microsoft/vcpkg/tree/master/ports/tgui) (unofficial, contains SFML_GRAPHICS and SDL_RENDERER backend)  
**PPA** (Ubuntu Linux): [libtgui-1.0-dev](https://launchpad.net/~texus/+archive/ubuntu/tgui) (official, contains all backends)  
**brew** (macOS): [tgui](https://formulae.brew.sh/formula/tgui) (unofficial, uses SFML_GRAPHICS backend)  
**MSYS2** (Windows): [mingw-w64-tgui](https://packages.msys2.org/base/mingw-w64-tgui) (unofficial, uses SFML_GRAPHICS backend)  

Language bindings:  
**C**: [CTGUI](https://github.com/texus/CTGUI)  
**C#**: [TGUI.Net](https://github.com/texus/TGUI.Net)  
**Ruby**: [white_gold](https://github.com/lpogic/white_gold)  
**Ada**: [ATGUI](https://github.com/mgrojo/ATGUI)  

### TGUI 0.9.5
**Previous version**. Minimum supported compilers: GCC 5, Clang 4, VS2017

{% include button.ext text="Source code" link="https://github.com/texus/TGUI/archive/v0.9.5.zip" style="White" %}

Precompiled libraries for Windows with SFML backend: (requires matching libraries from [SFML 2.6.2](https://www.sfml-dev.org/download/sfml/2.6.2/))

<table>
<tr>
  <td><p>{% include button.ext text="Visual Studio (2017-2022) - 32bit" link="https://github.com/texus/TGUI/releases/download/v0.9.5/TGUI-0.9.5-VisualStudio-32bit-for-SFML-2.6.2.zip" style="White DownloadBtnInTable" %}</p></td>
  <td><p>(Use with "Visual C++ 15 (2017) - 32-bit" SFML libs)</p></td>
</tr>
<tr>
  <td><p>{% include button.ext text="Visual Studio (2017-2022) - 64bit" link="https://github.com/texus/TGUI/releases/download/v0.9.5/TGUI-0.9.5-VisualStudio-64bit-for-SFML-2.6.2.zip" style="White DownloadBtnInTable" %}</p></td>
  <td><p>(Use with "Visual C++ 15 (2017) - 64-bit" SFML libs)</p></td>
</tr>
<tr>
  <td><p>{% include button.ext text="MinGW-w64 13.1.0 - i686&#8209;posix&#8209;dwarf (32bit)" link="https://github.com/texus/TGUI/releases/download/v0.9.5/TGUI-0.9.5-mingw-13.1.0-32bit-for-SFML-2.6.2.zip" style="White DownloadBtnInTable" %}</p></td>
  <td><p>{% include button.ext text="MinGW-w64 13.1.0 - x86_64&#8209;posix&#8209;seh (64bit)" link="https://github.com/texus/TGUI/releases/download/v0.9.5/TGUI-0.9.5-mingw-13.1.0-64bit-for-SFML-2.6.2.zip" style="White DownloadBtnInTable" %}</p></td>
</tr>
</table>

Packages:  
**PPA** (Ubuntu Linux): [libtgui-0.9-dev](https://launchpad.net/~texus/+archive/ubuntu/tgui) (official, uses SFML backend)  

### TGUI 0.8.9
**Old version**. Minimum supported compilers: GCC 4.9, Clang 3.6, VS2015

{% include button.ext text="Source code" link="https://github.com/texus/TGUI/archive/v0.8.9.zip" style="White" %}

### Older versions

Older versions are deprecated but can still be found in the [GitHub releases](https://github.com/texus/TGUI/releases).

