---
layout: page
title: Linux
breadcrumb: linux
---

### Getting TGUI

#### Ubuntu
You can install tgui by adding the [PPA](https://launchpad.net/~texus/+archive/ubuntu/tgui/) to your system and install the "libtgui-1.0-dev" package:
```bash
sudo add-apt-repository ppa:texus/tgui
sudo apt-get update
sudo apt-get install libtgui-1.0-dev
```

#### Arch Linux
You can download, build and install the latest tgui version with the "tgui-git" package from the [AUR](https://aur.archlinux.org/packages/tgui-git/). There are multiple ways to do this, here is one:
```bash
yay -S tgui-git
```

#### Other distros

You can download the source code from the [download page](/download) and follow the ['building with CMake' tutorial](../cmake) for a guide on how to build TGUI.  
After you ran `make` like explained in the tutorial, you should call `sudo make install` to install the libraries which you just built.


### Adding TGUI to your project if you compile from the terminal

To link to the TGUI libraries you only need to add `-ltgui` to your compiler command.  
```bash
g++ main.cpp -ltgui -lsfml-graphics -lsfml-window -lsfml-system -o program
```


### Adding TGUI to your CodeBlocks project

Open the "Project build options" and choose whether you want to change Debug or Release target settings. You can click on the name of your project (here TGUI-Test) and set settings that apply to both Debug and Release.  
[![CodeBlocks Project Build Options](/resources/Tutorials/0.9/LinuxCodeBlocksProjectBuildOptions.png){:width="684" height="414"}](/resources/Tutorials/0.9/LinuxCodeBlocksProjectBuildOptions.png)

Add `tgui` to the "Link libraries" in the "Linker settings" tab.
[![CodeBlocks Linker Settings](/resources/Tutorials/0.9/LinuxCodeBlocksProjectBuildOptionsLinkerSettings.png){:width="736" height="308"}](/resources/Tutorials/0.9/LinuxCodeBlocksProjectBuildOptionsLinkerSettings.png)


### Potential error

If you get the following error when running your program then you did install TGUI correctly, but it is not in the shared library cache.  
`/pathname/of/program: error while loading shared libraries: libtgui.so: cannot open shared object file: no such file or directory`

Try to open the terminal and execute `sudo ldconfig`.

If that doesn’t solve it then make sure the library was installed to a place where your linux distro searches for it.  
The easiest workaround is probably to add a new line containing `/usr/local/lib/` (and nothing more) at the end of /etc/ld.so.conf and then run `sudo ldconfig` again.
