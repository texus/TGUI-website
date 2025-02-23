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

A minimal main loop would look like this (with SFML 2):
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

To draw all widgets in the gui, you need to call `gui.draw()` once per frame, between the calls to `clear()` and `display()`. All widgets are drawn at once, if you wish to render SFML contents inbetween TGUI widgets then you need to use a [Canvas widget](../canvas/) or create a [custom widget](../custom-widgets).

#### handleWindowEvents (requires SFML >= 3)

SFML 3 added `window.handleEvents` as alternative to the `window.pollEvent` function. While you can't use it directly, TGUI provides a `handleWindowEvents` function that will call it for you and still provide you a similar interface.

If all events need to be handled by both TGUI and your code then `gui.handleWindowEvents` is a drop-in replacement for `window.handleEvents`. Otherwise you can use an optional bool parameter or bool return to determine whether the event gets handled by TGUI or your code as explained in the example below:
```c++
gui.handleWindowEvents(
    // A function that returns nothing will be called after the gui has handled the event
    [](const sf::Event::Closed&) { },

    // The function can take an optional extra parameter that indicates whether the gui processed the event.
    // The value of the boolean is what gets returned by the handleEvent function that is called internally.
    [](sf::Event::TextEntered&, bool /*consumedByGui*/) { },

    // If you don't want the gui to process some events, you can let the function return a bool.
    // In this case, the function will be called earlier and the return value determines if TGUI handles the event or ignores it.
    // The handleEvent function will only be called when the function returns true.
    [](sf::Event::MouseMoved) { return false; },

    // Generic lambdas are also supported. Note that multiple matching functions can be executed for the same event,
    // so this generic lambda is still called for Closed events even though a lambda above is also executed for Closed events.
    [](auto&& event) {
        if constexpr (std::is_same_v<std::decay_t<decltype(event)>, sf::Event::MouseButtonReleased>)
            return false;
        else
            return true;
    }
);
```
