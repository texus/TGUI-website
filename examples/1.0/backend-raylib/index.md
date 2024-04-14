---
layout: page
title: "Example: RAYLIB backend"
breadcrumb: "raylib backend"
---

The code below shows the template that is used for the other examples when using the RAYLIB backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on raylib and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [RAYLIB backend tutorial](/tutorials/1.0/backend-raylib/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/raylib.hpp>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying raylib
void run_application()
{
    tgui::Gui gui;
    if (!runExample(gui))
        return;

    gui.mainLoop();
}

int main()
{
    SetTraceLogLevel(LOG_WARNING);

    InitWindow(800, 600, "TGUI example (RAYLIB)");
    SetTargetFPS(30);

    run_application();

    CloseWindow();
}
```
