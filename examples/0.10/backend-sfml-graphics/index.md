---
layout: page
title: "Example: SFML_GRAPHICS backend"
breadcrumb: sfml_graphics backend
---

The code below shows the template that is used for the other examples when using the SFML_GRAPHICS backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on SFML and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [SFML_GRAPHICS backend tutorial](/tutorials/0.10/backend-sfml-graphics/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SFML-Graphics.hpp>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

int main()
{
    sf::RenderWindow window{ {800, 600}, "TGUI example - SFML_GRAPHICS backend" };

    tgui::Gui gui{window};
    if (runExample(gui))
        gui.mainLoop();
}
```
