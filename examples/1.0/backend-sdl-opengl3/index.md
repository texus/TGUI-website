---
layout: page
title: "Example: SDL_OPENGL3 backend"
breadcrumb: "sdl_opengl3 backend"
---

The code below shows the template that is used for the other examples when using the SDL\_OPENGL3 backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on SDL and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [SDL_OPENGL3 backend tutorial](/tutorials/1.0/backend-sdl-opengl3/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SDL-OpenGL3.hpp>

#include <SDL.h>  // At least 2.0.6
#include <SDL_opengl.h>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying SDL
void run_application(SDL_Window* window)
{
    tgui::Gui gui{window};
    if (!runExample(gui))
        return;

    gui.mainLoop();
}

// Note that no error checking is performed on SDL initialization in this example code
int main()
{
    SDL_Init(SDL_INIT_VIDEO);

    // The SDL backend in TGUI requires at least OpenGL 3.3
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_PROFILE_MASK, SDL_GL_CONTEXT_PROFILE_CORE);
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_MAJOR_VERSION, 3);
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_MINOR_VERSION, 3);

    // TGUI requires a window created with the SDL_WINDOW_OPENGL flag and an OpenGL context
    SDL_Window* window = SDL_CreateWindow("TGUI example - SDL_OPENGL3 backend",
                                          SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED,
                                          800, 600,
                                          SDL_WINDOW_OPENGL | SDL_WINDOW_SHOWN);
    SDL_GLContext glContext = SDL_GL_CreateContext(window);

    glClearColor(0.8f, 0.8f, 0.8f, 1.f);

    run_application(window);

    // All TGUI resources must be destroyed before SDL is cleaned up
    SDL_GL_DeleteContext(glContext);
    SDL_DestroyWindow(window);
    SDL_Quit();
}
```
