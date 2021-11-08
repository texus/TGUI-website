---
layout: page
title: Minimal code with SFML_GRAPHICS backend
breadcrumb: minimal code with sfml_graphics
---

The following is the minimal code you need for an application using TGUI with the SFML_GRAPHICS backend. Each part of the code and some alternatives are explained below.
{% raw %}
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SFML-Graphics.hpp>

int main()
{
    sf::RenderWindow window{{800, 600}, "TGUI example - SFML_GRAPHICS backend"};
    tgui::Gui gui{window};
    gui.mainLoop(); // See below for how to use your own main loop
}
```
{% endraw %}


### Including TGUI

Since the TGUI library can contain multiple backends, you will always need to include the backend that you want to use. Other than that there is a TGUI.hpp file which includes everything else that you need. This is the easiest way to include TGUI:
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SFML-Graphics.hpp>
```

Alternatively, you can selectively include what you need. You will always need `TGUI/Core.hpp` and a backend, but widgets can be included individually:
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backend/SFML-Graphics.hpp>
#include <TGUI/Widgets/Button.hpp>
#include <TGUI/Widgets/CheckBox.hpp>
```


### Creating the Gui object

Only one gui object should be created for each SFML window. The gui needs to know which window to render to, so it takes a reference to the window as parameter to the constructor:
```c++
tgui::Gui gui{window};
```

The gui also has a default constructor. If you use it then you will however need to call setWindow before interacting with the gui object.
```c++
tgui::Gui gui;
gui.setWindow(window);
```

Note that the Gui object is used for global initialization and destruction in TGUI. **You MUST NOT create other TGUI objects before the Gui has been given a window and you MUST NOT use TGUI objects after the last Gui object is destroyed**.


### Main loop

Although TGUI provides a `gui.mainLoop()` function for convenience, in many cases you will need to use your own main loop (e.g. if you need to draw things other than the gui or run code that isn't event-based).

A minimal main loop would look like this:
```c++
while (window.isOpen())
{
    sf::Event event;
    while (window.pollEvent(event))
    {
        gui.handleEvent(event);

        if (event.type == sf::Event::Closed)
            window.close();
    }

    window.clear();

    gui.draw();

    window.display();
}
```

In your event loop, `gui.handleEvent(event)` is used to inform the gui about the event. The gui will make sure that the event ends up at the widget that needs it. If all widgets ignored the event then `handleEvent` will return `false`. This could be used to e.g. check if a mouse event was handled by the gui or should still be handled by your own code.

To draw all widgets in the gui, you need to call `gui.draw()` once per frame, between the calls to `clear()` and `display()`.


### Canvas

When calling `gui.draw()`, all (visible) widgets are drawn at once. If you wish to manually render something with SFML inbetween TGUI widgets (e.g. to the background of a child window) then you need to use the CanvasSFML widget. The canvas acts as a render target: you first render your SFML contents to the canvas and then when TGUI draws its widgets, it will draw the contents of the canvas widget to the screen.

Creating the canvas is done like any other widget:
```c++
auto canvas = tgui::CanvasSFML::create();
canvas->setSize({400, 300});
gui.add(canvas);
```

Rendering to the CanvasSFML widget is the same as rendering to an `sf::RenderTarget`:
```c++
canvas->clear();      // Erasing the contents
canvas->draw(sprite); // Drawing sf::Sprite
canvas->draw(text);   // Drawing sf::Text
canvas->display();    // Saving the drawing to the canvas
```
