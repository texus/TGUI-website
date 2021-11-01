---
layout: page
title: "Example: Blocking events outside child window"
breadcrumb: "blocking events outside child window"
---

When you open a child window, a message box or a file dialog, you may not want the user to click outside it until the window is closed.

A simple way to accomplish this is to render a Panel widget behind the window. Since you probably still want to see the contents behind the window, the background color should be set to transparent. I recommend using a semi-transparent color instead of fully transparent because it makes it more clear to the user that he has to interact with the window.

```c++
// Keep a global pointer to the gui to make it accessible everywhere.
// This is bad practice, but it keeps the example code short and simple.
tgui::GuiBase* guiPtr = nullptr;

void closeWindow()
{
    // Find the transparent panel and destroy it
    guiPtr->remove(guiPtr->get("TransparentBackground"));
}

void openWindow()
{
    // Create the transparent panel that covers the existing contents
    auto panel = tgui::Panel::create({"100%", "100%"});
    panel->getRenderer()->setBackgroundColor({0, 0, 0, 175});
    guiPtr->add(panel, "TransparentBackground");

    // Create the child window and make sure to add it to the panel instead of the gui
    auto window = tgui::ChildWindow::create();
    panel->add(window);

    // Remove the panel when the window is closed
    window->onClose([]{ closeWindow(); });
}

bool runExample(tgui::GuiBase& gui)
{
    guiPtr = &gui;

    // Create a button to open the window.
    // Once the window is open you won't be able to press it again until the window closes.
    auto button = tgui::Button::create("Open window");
    button->onPress([]{ openWindow(); });
    gui.add(button);
    return true;
}
```
