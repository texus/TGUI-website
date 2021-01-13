---
layout: page
title: Minimal code with SDL
breadcrumb: minimal code with SDL
---

If you build TGUI yourself with CMake then you have the option to build the SDL backend. When using a TGUI library that includes this backend, the following code can be used to use TGUI in an SDL application. The code contains some comments with explanations, but some extra information are also provided below the example code.
```c++
#include <SDL.h>            // SDL 2.0 required
#include <SDL_ttf.h>        // Minimum 2.0.14 (released Feb 2016)
#include <SDL_opengl.h>

#include <TGUI/Core.hpp>
#include <TGUI/Backends/SDL.hpp>
#include <TGUI/AllWidgets.hpp>

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying SDL
void run_application(SDL_Window* window)
{
    tgui::GuiSDL gui{window};
    gui.mainLoop(); // See below for how to use your own main loop
}

// Note that no error checking is performed on SDL initialization in this example code
int main()
{
    SDL_Init(SDL_INIT_VIDEO);

    // The SDL backend in TGUI currently requires at least OpenGL 4.1 (or OpenGL ES 3.0 on Android)
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_MAJOR_VERSION, 4);
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_MINOR_VERSION, 1);
    SDL_GL_SetAttribute(SDL_GL_CONTEXT_PROFILE_MASK, SDL_GL_CONTEXT_PROFILE_CORE);

    // TGUI requires a window created with the SDL_WINDOW_OPENGL flag and an OpenGL context
    SDL_Window* window = SDL_CreateWindow("TGUI window with SDL",
                                          SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED,
                                          800, 600,
                                          SDL_WINDOW_OPENGL | SDL_WINDOW_SHOWN);
    SDL_GLContext glContext = SDL_GL_CreateContext(window);

    glClearColor(0.8f, 0.8f, 0.8f, 1.f);

    // SDL_TTF needs to be initialized before using TGUI
    TTF_Init();

    run_application(window);

    // All TGUI resources must be destroyed before SDL_TTF is cleaned up
    TTF_Quit();

    SDL_GL_DeleteContext(glContext);
    SDL_DestroyWindow(window);
    SDL_Quit();
}

```


### Including TGUI

You will always need `TGUI/Core.hpp` and `TGUI/Backends/SDL.hpp`, but instead of including `TGUI/AllWidgets.hpp` you could also include the widgets you need selectively:
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backends/SDL.hpp>
#include <TGUI/Widgets/Button.hpp>
#include <TGUI/Widgets/CheckBox.hpp>
```


### Creating the gui object

Only one gui object should be created for each SDL window. Note that the SDL backend is currently limited to only a single window in your application.

The gui needs to know which window to render to, so it takes a pointer to the window as parameter to the constructor:
```c++
tgui::GuiSDL gui{window};
```

The gui also has a default constructor. If you use it then you will however need to call setWindow before interacting with the gui object.
```c++
tgui::GuiSDL gui;
gui.setWindow(window);
```


### Main loop

Although TGUI provides a `gui.mainLoop()` function for convenience, in many cases you will need to use your own main loop (e.g. if you need to draw things other than the gui or run code that isn't event-based).

A minimal main loop would look like this:
```c++
bool quit = false;
while (!quit)
{
    SDL_Event event;
    while (SDL_PollEvent(&event) != 0)
    {
        gui.handleEvent(event); // Pass the event to the gui

        if (event.type == SDL_QUIT)
            quit = true;
    }

    glClear(GL_COLOR_BUFFER_BIT);

    gui.draw(); // Tell the gui to draw all widgets

    SDL_GL_SwapWindow(window);
}
```

In your event loop, `gui.handleEvent(event)` is used to inform the gui about the event. The gui will make sure that the event ends up at the widget that needs it. If all widgets ignored the event then `handleEvent` will return `false`. This could be used to e.g. check if a mouse event was handled by the gui or should still be handled by your own code.

To draw all widgets in the gui, you need to call `gui.draw()` once per frame. All widgets are drawn at once, the SDL backend currently doesn't provide a way to render SDL contents inbetween TGUI widgets (without creating a custom widget).


### Multiple windows

You can use TGUI for multiple SDL windows, but you have to make sure that the correct OpenGL context is activated. If you want to access the gui, you first have to call `SDL_GL_MakeCurrent` with the associated window and opengl context for that gui object.
```c++
const Uint32 windowId1 = SDL_GetWindowID(window1);
const Uint32 windowId2 = SDL_GetWindowID(window2);

SDL_GL_MakeCurrent(window1, glContext1);
tgui::GuiSDL gui1(window1);

SDL_GL_MakeCurrent(window2, glContext2);
tgui::GuiSDL gui2(window2);

bool quit = false;
while (!quit)
{
    SDL_Event event;
    while (SDL_PollEvent(&event) != 0)
    {
        // Make sure the application stops when all windows are closed
        if (event.type == SDL_QUIT)
            quit = true;

        // Close the window when its close button is pressed
        else if ((event.type == SDL_WINDOWEVENT) && (event.window.event == SDL_WINDOWEVENT_CLOSE))
        {
            if (window1 && (event.window.windowID == windowId1))
            {
                SDL_DestroyWindow(window1);
                window1 = nullptr;
            }
            else if (window2 && (event.window.windowID == windowId2))
            {
                SDL_DestroyWindow(window2);
                window2 = nullptr;
            }
        }

        // Pass the event to the correct gui
        if (window1 && (event.window.windowID == windowId1))
        {
            SDL_GL_MakeCurrent(window1, glContext1);
            gui1.handleEvent(event);
        }
        else if (window2 && (event.window.windowID == windowId2))
        {
            SDL_GL_MakeCurrent(window2, glContext2);
            gui2.handleEvent(event);
        }
    }

    // Draw the contents of the first window (if it is still open)
    if (window1)
    {
        SDL_GL_MakeCurrent(window1, glContext1);
        glClear(GL_COLOR_BUFFER_BIT);
        gui1.draw();
        SDL_GL_SwapWindow(window1);
    }

    // Draw the contents of the second window (if it is still open)
    if (window2)
    {
        SDL_GL_MakeCurrent(window2, glContext2);
        glClear(GL_COLOR_BUFFER_BIT);
        gui2.draw();
        SDL_GL_SwapWindow(window2);
    }
}
```
