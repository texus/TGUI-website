---
layout: page
title: "Example: Pop-up menu"
breadcrumb: "pop-up menu"
---

TGUI currently has no proper support for right click pop-up menus (a.k.a. context menus).

You can however create such functionality yourself. The list itself works just like a ListBox widget, but the tricky part is handling the right click event. The code below shows one way to do this. There might be an alternative solution by writing a custom widget, but this was never investigated as this tutorial was ported from a TGUI 0.7 tutorial where right click events for custom widgets weren't supported yet.

This example code isn't backend-independent and uses SFML directly because it has to process the right click event before it reaches TGUI.
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backends/SFML.hpp>
#include <TGUI/AllWidgets.hpp>

tgui::ListBox::Ptr popupMenu;

// Called when selecting an item in the pop-up menu
void popupMenuCallback(tgui::String item)
{
    std::cout << item << std::endl;
}

// Called when a right click is detected while popup menu isn't shown
void rightClickCallback(tgui::BackendGui& gui, tgui::Vector2f position)
{
    popupMenu = tgui::ListBox::create();
    popupMenu->addItem("Option 1");
    popupMenu->addItem("Option 2");
    popupMenu->addItem("Option 3");
    popupMenu->addItem("Option 4");
    popupMenu->setItemHeight(20);
    popupMenu->setPosition(position);
    popupMenu->setSize(120, popupMenu->getItemHeight() * popupMenu->getItemCount());
    popupMenu->onItemSelect(&popupMenuCallback);
    gui.add(popupMenu);
}

int main()
{
    sf::RenderWindow window(sf::VideoMode(800, 600), "TGUI window");
    tgui::Gui gui(window);

    while (window.isOpen())
    {
        sf::Event event;
        while (window.pollEvent(event))
        {
            if (event.type == sf::Event::Closed)
                window.close();

            // Check if there is a pop-up menu
            if (popupMenu)
            {
                // When mouse is released, remove the pop-up menu
                if (event.type == sf::Event::MouseButtonReleased && event.mouseButton.button == sf::Mouse::Left)
                {
                    gui.remove(popupMenu);
                    popupMenu = nullptr;
                }

                // When mouse is pressed, remove the pop-up menu only when the mouse is not on top of the menu
                if (event.type == sf::Event::MouseButtonPressed)
                {
                    if (!popupMenu->isMouseOnWidget(tgui::Vector2f(event.mouseButton.x, event.mouseButton.y)))
                    {
                        gui.remove(popupMenu);
                        popupMenu = nullptr;
                    }
                }
            }

            // Perhaps we have to open a menu
            else if (event.type == sf::Event::MouseButtonPressed && event.mouseButton.button == sf::Mouse::Right)
            {
                rightClickCallback(gui, tgui::Vector2f(event.mouseButton.x, event.mouseButton.y));
            }

            gui.handleEvent(event);
        }

        window.clear();
        gui.draw();
        window.display();
    }
}
```
