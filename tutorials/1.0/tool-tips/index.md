---
layout: page
title: Tool Tips
breadcrumb: tool tips
---

### Normal tool tip

Tool tips are usually just labels, which is exactly what you get if you use the renderer of one of the themes shipped with TGUI.
```c++
auto toolTip = tgui::Label::create("world");
toolTip->setRenderer(theme.getRenderer("ToolTip"));
button->setToolTip(toolTip);
```

<img src="/resources/Tutorials/ToolTipLabel.jpg" alt="Tool Tip Label" width="181" height="77" />

Note that unlike other widgets, the tool tip should NOT be added to the gui or any other container. The ownership of the tool tip will be automatically managed by calling setToolTip.


### Special tool tip

The tool tip does not have to be a label, it can be any widget. Here I used a Picture but you could also use a Panel and make the tool tip as complex as you want.
```c++
auto toolTip = tgui::Picture::create("image.png");
button->setToolTip(toolTip);
```

<img src="/resources/Tutorials/ToolTipPicture.jpg" alt="Tool Tip Picture" width="177" height="99" />


### Properties

There also exists a ToolTip class in which you find static function to change the time before the tool tip shows up and the distance between the mouse and the tool tip. Setting the display time to 0 will cause the tool tip to always be visible when the mouse is on top of the widget.
```c++
tgui::ToolTip::setTimeToDisplay(300); // Passing "300" is equivalent to passing "std::chrono::milliseconds(300)"
tgui::ToolTip::setDistanceToMouse({4, 8});
```
