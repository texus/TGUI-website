---
layout: page
title: "Example: SFML_OPENGL3 backend"
breadcrumb: "sfml_opengl3 backend"
---

The code below shows the template that is used for the other examples when using the SFML_OPENGL3 backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on SFML and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [SFML_OPENGL3 backend tutorial](/tutorials/1.0/backend-sfml-opengl3/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SFML-OpenGL3.hpp>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

int main()
{
    // The OpenGL renderer backend in TGUI requires at least OpenGL 3.3
    sf::ContextSettings settings;
    settings.attributeFlags = sf::ContextSettings::Attribute::Core;
    settings.majorVersion = 3;
    settings.minorVersion = 3;

    sf::Window window{ {800, 600}, "TGUI example - SFML_OPENGL3 backend", sf::Style::Default, settings };

    tgui::Gui gui{window};
    if (runExample(gui))
        gui.mainLoop();
}
```
