---
layout: page
title: Changing mouse cursor
breadcrumb: mouse cursor
---

This tutorial will show how to change the mouse cursor.

A list of supported mouse cursors can be found in the [Cursor::Type documentation](https://tgui.eu/documentation/0.9/classtgui_1_1Cursor.html#a4e9a3c57acd4bf2b261aa2fe77f4d2f5).

Note that resizable child windows automatically change the cursor when the mouse is on top of one of the borders, no additional code is required for that case.

### Cursor per widget

For each widget you can specify which mouse cursor you want to show while the mouse is on top of the widget.

To display the `I` text edit icon when the mouse is on top of an edit box, you need the following code:
```c++
editBox->setMouseCursor(tgui::Cursor::Type::Text);
```

If the edit box was part of a panel and you chose a different cursor for the panel then the I-beam cursor will still be used for the edit box, the other cursor would only be shown when the mouse is anywhere else on the panel.

### Setting global cursor

The gui object provides a `setOverrideMouseCursor` function which changes the cursor everywhere. The cursor will be shown until `restoreOverrideMouseCursor` is called. While the cursor is overriden, the per-widget cursor is ignored and the cursor won't change for resizable child windows.

If you call `setOverrideMouseCursor` multiple times then the cursors are stacked, you need to call `restoreOverrideMouseCursor` for every call to `setOverrideMouseCursor`.
```c++
gui.setOverrideMouseCursor(tgui::Cursor::Type::Hand); // Cursor becomes "hand"
gui.setOverrideMouseCursor(tgui::Cursor::Type::Crosshair); // Cursor becomes "+" icon
gui.restoreOverrideMouseCursor(); // Cursor becomes "hand" again
gui.restoreOverrideMouseCursor(); // Cursor becomes a normal arrow again
```

Note that the mouse cursor is specific to the window. If the mouse leaves the window then it is a normal arrow again, but it will turn back into the requested cursor as soon as the mouse re-enters the window.

### Specifying custom cursor image

By default the system mouse cursors are used. Each cursor can however be replaced by a bitmap.

The setStyle function takes 4 arguments:
- Which mouse cursor to change
- A pointer to RGBA pixel data (4\*width\*height bytes)
- The width and height of the cursor
- The offset of the hotspot, which pixel is centered at the mouse position

```c++
tgui::Cursor::setStyle(tgui::Cursor::Type::Arrow, pixels, {14,22}, {0,0});
```

If you need to load the image from a file then you could use TGUI's internal image loader to get the pixel data:
```c++
#include <TGUI/Loading/ImageLoader.hpp>

tgui::Vector2u imageSize;
std::unique_ptr<std::uint8_t[]> imagePixels = tgui::ImageLoader::loadFromFile("image.png", imageSize);
if (imagePixels)
    tgui::Cursor::setStyle(tgui::Cursor::Type::Arrow, imagePixels.get(), imageSize, {0,0});
```
