---
layout: page
title: "Example: SDL_GPU backend"
breadcrumb: "SDL_GPU backend"
---

The code below shows the template that is used for the other examples when using the SDL\_GPU backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on SDL and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [SDL_GPU backend tutorial](/tutorials/1.0/backend-sdl-gpu/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SDL-GPU.hpp>
#include <SDL3/SDL_main.h>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying SDL
void run_application(SDL_Window* window, SDL_GPUDevice* device)
{
    tgui::Gui gui(window, device);
    if (!runExample(gui))
        return;

    gui.mainLoop();
}

// Note that no error checking is performed on SDL initialization in this example code
int main(int, char **)
{
    SDL_Init(SDL_INIT_VIDEO);

    SDL_GPUDevice* device = SDL_CreateGPUDevice(SDL_GPU_SHADERFORMAT_SPIRV | SDL_GPU_SHADERFORMAT_DXIL | SDL_GPU_SHADERFORMAT_MSL, false, nullptr);
    SDL_Window* window = SDL_CreateWindow("TGUI example (SDL-GPU)", 800, 600, SDL_WINDOW_RESIZABLE);
    SDL_ClaimWindowForGPUDevice(device, window);

    // SDL_ttf needs to be initialized before using TGUI
    TTF_Init();

    run_application(window, device);

    // Note that all TGUI resources must be destroyed before SDL_ttf is cleaned up
    TTF_Quit();

    SDL_ReleaseWindowFromGPUDevice(device, window);
	SDL_DestroyWindow(window);
	SDL_DestroyGPUDevice(device);
    SDL_Quit();
    return 0;
}
```
