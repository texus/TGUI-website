---
layout: page
title: "Switching between menu screens / forms"
breadcrumb: "switching screens"
---

When your program consists of multiple screens then you need a method to replace your gui widgets with different ones. There are multiple ways to do this, you can either create the widgets when switching or you can load all widgets in advance so that you just have to hide one form and show another one.

### 1. Removing all widgets and creating new ones

The first method is the most straigtforward one, simly call `removeAllWidgets` and create all the new widgets that need to be shown.
```c++
void showScreen1()
{
    gui.removeAllWidgets();

    auto button1 = tgui::Button::create("Switch to sceen 2");
    button1->onPress([]{ showScreen2(); });
    gui.add(button1);
}

void showScreen2()
{
    gui.removeAllWidgets();

    auto button2 = tgui::Button::create("Switch to sceen 1");
    button2->onPress([]{ showScreen1(); });
    gui.add(button2);
}
```

### 2. Loading a new form file

If you wish to load widgets from a form file (which can be created with the Gui Builder) instead of manually creating them in c++ then you can use the `loadWidgetsFromFile` function to replace all widgets.
```c++
void showScreen1()
{
    gui.loadWidgetsFromFile("form1.txt");

    gui.get<tgui::Button>("Button1")->onPress([]{ showScreen2(); });
}

void showScreen2()
{
    gui.loadWidgetsFromFile("form2.txt");

    gui.get<tgui::Button>("Button2")->onPress([]{ showScreen1(); });
}
```

### 3. Loading upfront and swapping group

Loading a handful of widgets shouldn't be slow, but if you want to make sure that the transition happens without any delay or that there can't be any errors during a screen change then you can load everything upfront. Group widgets are ideal to bundle all widgets of a screen: they are invisible (i.e. they have no background color like Panel) and they fill the entire parent area if you don't manually specify a size. Once all widgets are loaded into groups, all you need to do when switching screens is to hide one group and show the other.
```c++
void init()
{
    auto group1 = tgui::Group::create();
    group1->loadWidgetsFromFile("form1.txt");
    group1->get<tgui::Button>("Button1")->onPress([]{ showScreen2(); });
    gui.add(group1);

    auto group2 = tgui::Group::create();
    group2->loadWidgetsFromFile("form2.txt");
    group2->get<tgui::Button>("Button2")->onPress([]{ showScreen1(); });
    gui.add(group2);
}

void showScreen1()
{
    group1->setVisible(false);
    group2->setVisible(true);
}

void showScreen2()
{
    group1->setVisible(true);
    group2->setVisible(false);
}
```

### Destroying widgets from inside callback

The first two methods have a major drawback compared to the third method: they destroy the button that triggered the signal from inside the callback function. This is allowed and the code works because Button::onPress intentionally allows the button to be destroyed. For all other signals or widgets though, this is not guaranteed and some signals might cause crashes when trying to destroy the widget.

If you must destroy any other kind of widget from inside its callback function then you should schedule the destruction to a later moment. To do this, TGUI supports timers and the ability to create a timer that runs as soon as possible. The following code will internally create a timer that makes sure that `showScreen1` gets called before drawing the next frame.
```c++
tgui::Timer::scheduleCallback(&showScreen1);
```

We can use this from inside a signal callback to safely remove the picture once it is clicked. Instead of calling `showScreen1` immediately, the function will instead be called after the onClick signal is handled and thus calling `removeAllWidgets` no longer causes any issues.
```c++
picture->onClick([]{ tgui::Time::scheduleCallback(&showScreen1); });
```
