---
layout: page
title: "Example: Tabs"
breadcrumb: "tabs"
---

TGUI provides a Tabs widget which contains just the tabs without any contents related to each tab. The TabContainer widget on the other hand combines the Tabs with Panel widgets so that a different panel is shown depending on which tab is selected.

### TabContainer widget

```c++
int runExample(tgui::BackendGui& gui)
{
    // Create the tabs and their panels
    tgui::TabContainer::Ptr tabContainer = tgui::TabContainer::create();
    tgui::Panel::Ptr panel1 = tabContainer->addTab("First");
    tgui::Panel::Ptr panel2 = tabContainer->addTab("Second");
    tabContainer->setPosition(20, 20);
    tabContainer->setTabsHeight(30);
    tabContainer->setSize(400, 330); // Panels are 400x300 while tab height is 30
    gui.add(tabContainer);

    // Add background images to the panels
    panel1->add(tgui::Picture::create("Image1.png"));
    panel2->add(tgui::Picture::create("Image2.png"));

    // Select the first tab and show the first panel
    tabContainer->select(0);
    return true;
}
```

### Tabs widget

The example below does the same as the above example, but we have to manage the panels ourselves here since we use the Tabs widget directly. You get much more control with this method though: the panels could be positioned somewhere else, or there don't even need to be any panels and the tabs e.g. act as alternative to radio buttons.

```c++
void onTabSelected(tgui::BackendGui& gui, tgui::String selectedTab)
{
    // Show a different panel depending on which tab is selected
    if (selectedTab == "First")
    {
        gui.get("FirstPanel")->setVisible(true);
        gui.get("SecondPanel")->setVisible(false);
    }
    else if (selectedTab == "Second")
    {
        gui.get("FirstPanel")->setVisible(false);
        gui.get("SecondPanel")->setVisible(true);
    }
}

int runExample(tgui::BackendGui& gui)
{
    tgui::Tabs::Ptr tabs = tgui::Tabs::create();
    tabs->add("First");
    tabs->add("Second");
    tabs->setPosition(20, 20);
    tabs->setSize(400, 30);
    gui.add(tabs);

    // Create the panel that will be shown when the first tab is selected
    tgui::Panel::Ptr panel1 = tgui::Panel::create();
    panel1->setSize(400, 300);
    panel1->setPosition({tgui::bindLeft(tabs), tgui::bindBottom(tabs)});
    gui.add(panel1, "FirstPanel");

    // Create the panel that will be shown when the second tab is selected (by copying of first one)
    tgui::Panel::Ptr panel2 = tgui::Panel::copy(panel1);
    gui.add(panel2, "SecondPanel");

    // Add background images to the panels
    panel1->add(tgui::Picture::create("Image1.png"));
    panel2->add(tgui::Picture::create("Image2.png"));

    // Select the first tab and only show the first panel
    tabs->select("First");
    panel1->setVisible(true);
    panel2->setVisible(false);

    // Subscribe callback when another tab is selected (passing reference to the gui as first parameter)
    tabs->onTabSelect(&onTabSelected, std::ref(gui));

    return true;
}
```
