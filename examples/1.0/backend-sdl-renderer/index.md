---
layout: page
title: "Example: SDL_RENDERER backend"
breadcrumb: "SDL_RENDERER backend"
---

The code below shows the template that is used for the other examples when using the SDL\_RENDERER backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on SDL and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [SDL_RENDERER backend tutorial](/tutorials/1.0/backend-sdl-renderer/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SDL-Renderer.hpp>

#include <SDL.h>      // At least 2.0.18
#include <SDL_ttf.h>  // At least 2.0.14

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying SDL
void run_application(SDL_Window* window, SDL_Renderer* renderer)
{
    tgui::Gui gui{window, renderer};
    if (!runExample(gui))
        return;

    gui.mainLoop();
}

// Note that no error checking is performed on SDL initialization in this example code
int main(int, char**)
{
    SDL_Init(SDL_INIT_VIDEO);

    SDL_Window* window = SDL_CreateWindow("TGUI example - SDL_RENDERER backend",
                                          SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED,
                                          800, 600,
                                          SDL_WINDOW_SHOWN);
    SDL_Renderer* renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);

    // SDL_TTF (and SDL) needs to be initialized before using TGUI
    TTF_Init();

    run_application(window, renderer);

    // All TGUI resources must be destroyed before SDL_TTF (and SDL) is cleaned up
    TTF_Quit();

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
    return 0;
}
```
