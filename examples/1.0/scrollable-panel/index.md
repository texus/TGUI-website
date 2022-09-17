---
layout: page
title: "Example: Scrollable panel"
breadcrumb: "scrollable panel"
---

Although TGUI provides a Scrollbar widget, the easiest method to add a scrollbar is actually to use the ScrollablePanel widget.

The ScrollablePanel has a size and content size. Once the content size becomes larger than the size, scrollbars will become visible.

```c++
bool runExample(tgui::BackendGui& gui)
{
    // Create the panel that will take up 400x500 pixels on the screen.
    // Once the contents of the panel exceeds this size, scrollbars will become visible.
    auto panel = tgui::ScrollablePanel::create();
    panel->setPosition(100, 50);
    panel->setSize(400, 500);
    gui.add(panel);

    // Create some pictures to place inside the scrollable panel
    for (unsigned int i = 0; i < 3; ++i)
    {
        auto pic = tgui::Picture::create("Image" + tgui::String(i+1) + ".png");
        pic->setSize(400, 300);
        pic->setPosition(0, i * 300);
        panel->add(pic);
    }

    // The scrollable area / content size is now 400x900 because of the pictures inside it.
    // If you wish to manually specify the size then you can call the setContentSize function.

    // Hide the horizontal scrollbar. Since the pictures have the same width as the panel,
    // the pictures won't fit inside the panel anymore when the vertical scrollbar is visible.
    // Note that by removing the horizontal scrollbar you won't be able to see the small part
    // of the picture that ends up below the vertical scrollbar.
    panel->setHorizontalScrollbarPolicy(tgui::Scrollbar::Policy::Never);
    return true;
}
```
