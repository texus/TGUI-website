---
layout: page
title: Minimal code with RAYLIB backend
breadcrumb: minimal code with raylib
---

The following is the minimal code you need for an application using TGUI with the RAYLIB backend. Each part of the code and some alternatives are explained below.
{% raw %}
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/raylib.hpp>

void run_application()
{
    tgui::Gui gui;
    gui.mainLoop();
}

int main()
{
    SetTraceLogLevel(LOG_WARNING);

    InitWindow(800, 600, "TGUI example - RAYLIB backend");
    SetTargetFPS(30);

    run_application();

    // All TGUI resources must be destroyed before raylib is cleaned up
    CloseWindow();
}
```
{% endraw %}


### Including TGUI

Since the TGUI library can contain multiple backends, you will always need to include the backend that you want to use. Other than that there is a TGUI.hpp file which includes everything else that you need. This is the easiest way to include TGUI:
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/raylib.hpp>
```

Alternatively, you can selectively include what you need. You will always need `TGUI/Core.hpp` and a backend, but widgets can be included individually:
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backend/raylib.hpp>
#include <TGUI/Widgets/Button.hpp>
#include <TGUI/Widgets/CheckBox.hpp>
```


### Creating the Gui object

Because raylib only supports a single window, the gui will automatically attach to it when constructed. You must however make sure to only create the gui AFTER `InitWindow` was called.
```c++
tgui::Gui gui;
```

Note that the Gui object is used for global initialization and destruction in TGUI. **You MUST NOT create other TGUI objects before the Gui has been created and you MUST NOT use TGUI objects after the last Gui object is destroyed**.


### Main loop

Although TGUI provides a `gui.mainLoop()` function for convenience, in many cases you will need to use your own main loop (e.g. if you need to draw things other than the gui or run code that isn't event-based).

Because `GetCharPressed` and `GetKeyPressed` remove data from a queue, you need to call them yourself and pass the values to the gui. That way both your own code and TGUI have access to the key events. All other events will be handled by TGUI by calling `gui.handleEvents()`. A minimal main loop would thus look like this:
```c++
while (!WindowShouldClose())
{
    gui.handleEvents(); // Handles all non-keyboard events

    int pressedChar = GetCharPressed();
    while (pressedChar)
    {
        gui.handleCharPressed(pressedChar);
        pressedChar = GetCharPressed();
    }

    int pressedKey = GetKeyPressed();
    while (pressedKey)
    {
        gui.handleKeyPressed(pressedKey);
        pressedKey = GetKeyPressed();
    }

    BeginDrawing();
    ClearBackground({240, 240, 240, 255});
    gui.draw();
    EndDrawing();
}
```

To draw all widgets in the gui, you need to call `gui.draw()` once per frame, between the calls to `BeginDrawing` and `EndDrawing`. All widgets are drawn at once, if you wish to render raylib contents inbetween TGUI widgets then you need to use a [Canvas widget](../canvas/) or create a [custom widget](../custom-widgets).
