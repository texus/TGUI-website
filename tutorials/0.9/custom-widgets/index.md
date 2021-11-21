---
layout: page
title: Custom widgets
breadcrumb: custom widgets
---

The amount of work required depends on how much you can reuse from existing widgets. The different "approaches" below are sorted by least amount of work first.

### Basic functionality

Before discussing the different approaches, let's first have a look at the functions that always need to exist in the custom widget. Technically most of them aren't really required, but these exist in all TGUI widgets and it is good practice to have these functions in custom widgets as well.

```c++
class MyCustomWidget : public tgui::XXX /* base type depends on approach */
{
public:
    // Add a Ptr typedef so that "MyCustomWidget::Ptr" has the correct type
    typedef std::shared_ptr<MyCustomWidget> Ptr;
    typedef std::shared_ptr<const MyCustomWidget> ConstPtr;

    // Add a constructor that sets the type name. This is the string that would be returned when calling getWidgetType().
    // We assume that we can keep the renderer of the base class in this code. See the "Changing renderer" section later
    // for an explanation on how to make changes to the renderer as well.
    MyCustomWidget(const char* typeName = "MyCustomWidget", bool initRenderer = true) :
        tgui::XXX(typeName, initRenderer) /* base type depends on approach */
    {
    }

    // Copy constructors, assignment operators and destructor can be added if required for the members that
    // you add to the custom widget. These functions are not required by default.

    // Every widget needs a static create function which creates the widget and returns a smart pointer to it.
    // This function can have any parameters you want.
    static MyCustomWidget::Ptr create()
    {
        return std::make_shared<MyCustomWidget>();
    }

    // Every widget has a static method to copy it
    static MyCustomWidget::Ptr copy(MyCustomWidget::ConstPtr widget)
    {
        if (widget)
            return std::static_pointer_cast<MyCustomWidget>(widget->clone());
        else
            return nullptr;
    }

protected:

    // Every widget needs a method to copy it
    Widget::Ptr clone() const override
    {
        return std::make_shared<MyCustomWidget>(*this);
    }
};
```

### Approach 1: extending existing widget

When you only need to add some additions to an existing widget, you can obviously simply inherit from that widget type.

```c++
class MyCustomButton : public tgui::Button
{
    // Add functions that are explained in "Basic functionality" section

    // Add whatever functionality you need
    void myNewFunction();
};
```

### Approach 2: combining widgets with SubwidgetContainer

Some widgets can be constructed by adding some existing widgets together and adding some glue between them. The SpinControl widget for example just combines an EditBox and SpinButton, while the TabContainer widget combines Tabs with Panel widgets.

If this is what you want then you can inherit from the SubwidgetContainer base class. The class has a protected m_container member to which you can add your subwidgets.

In this example code, the subwidgets are public which allows you to access them directly to e.g. change the renderer properties. Although making them private would be a better practice, it requires more work as you need additional functions to expose the required functionality on them.

```c++
class MyCustomWidget : public tgui::SubwidgetContainer
{
    // Add functions that are explained in "Basic functionality" section

    // The constructor should create the subwidgets and add them to the internal container
    MyCustomWidget(const char* typeName = "MyCustomWidget", bool initRenderer = true) :
        tgui::SubwidgetContainer(typeName, initRenderer)
    {
        subWidget1 = tgui::EditBox::create();
        subWidget2 = tgui::Button::create();

        // Give the subwidgets their size
        updateSize();
    }

    // When the user changes the size of this widget, we need to update the subwidgets
    void setSize(const tgui::Layout2d& size) override
    {
        // Make sure to actually update the container size first
        SubwidgetContainer::setSize(size);

        // Call our own function that repositions and resizes the subwidgets
        updateSize();
    }

    // Add the following line to allow the compiler to find the setSize(w, h) function,
    // which will simply call the setSize(size) function that was overridden above.
    using tgui::SubwidgetContainer::setSize;

private:
    
    // When the size changes, the subwidgets need to be repositioned and resized.
    // In this simple case we could have given the subwidgets a relative position and size
    // in the constructor so that we don't need a manually reposition/resize, but I'm
    // intentionally not doing that to show how to override setSize.
    void updateSize()
    {
        subWidget1->setSize({getSize().x * 0.8, getSize().y});
        subWidget2->setSize({getSize().x * 0.2, getSize().y});
        subWidget2->setPosition({getSize().x * 0.8, getSize().y});
    }

public:
    tgui::EditBox::Ptr subWidget1;
    tgui::Button::Ptr  subWidget2;
};
```

### Approach 3: inherit from Widget

To create a custom widget that isn't based on any of the existing widgets, you have to inherit directly from the Widget base class.

There are 2 functions that you must implement in your custom widget: `isMouseOnWidget()` and `draw()`.

```c++
class MyCustomWidget : public tgui::SubwidgetContainer
{
    // Add functions that are explained in "Basic functionality" section

    // The isMouseOnWidget function should return true if the mouse is on top of the widget
    bool isMouseOnWidget(tgui::Vector2f pos) const override
    {
        // The following assumes that the widget is rectangular and fills the entire size.
        // Note that "pos" is relative the the parent of the widget.
        return tgui::FloatRect{getPosition().x, getPosition().y, getSize().x, getSize().y}.contains(pos);
    }

    // The draw function will be called when it is time to render the widget to the screen
    void draw(tgui::BackendRenderTargetBase& target, tgui::RenderStates states) const override
    {
        // To render something you can call functions on the "target" object.
        // Note that the position in "states" is already relative to this widget,
        // so you no longer have to add getPosition() anywhere.

        // This example code will draw a red rectangle with blue borders of 2px around it.
        // The m_opacityCached variable is defined in the Widget base class and contains
        // the value of the Opacity property of the widget renderer.

        const tgui::Borders borders{2}; // Borders are 2 pixels thick on any side

        target.drawBorders(states, borders, getSize(), tgui::Color::applyOpacity(tgui::Color::Blue, m_opacityCached));

        states.transform.translate(borders.getOffset());

        target.drawFilledRect(states,
            {getSize().x - borders.getLeft() - borders.getRight(), getSize().y - borders.getTop() - borders.getBottom()},
            tgui::Color::applyOpacity(tgui::Color::Red, m_opacityCached));
    }
};
```

Unlike with the SubwidgetContainer which forwards all events to the correct subwidget, if you inherit from Widget directly then you will likely have to handle some events yourself. All event functions have a default implementation, so you only need to override the functions that you actually need. Here is a list of function signatures that you can add to your class:
```c++
void leftMousePressed(Vector2f pos) override;
void leftMouseReleased(Vector2f pos) override;
void rightMousePressed(Vector2f pos) override;
void rightMouseReleased(Vector2f pos) override;
void mouseMoved(Vector2f pos) override;
void keyPressed(const Event::KeyEvent& event) override;
void textEntered(char32_t key) override;
bool mouseWheelScrolled(float delta, Vector2f pos) override;
void mouseNoLongerOnWidget() override;
void leftMouseButtonNoLongerDown() override;
void rightMouseButtonNoLongerDown() override;
```

Some of those functions are self-explanatory, but there are a few things to note:
- The "pos" parameters are the mouse positions relative to the parent of the widget
- xxxMouseReleased is called when the mouse is released on top of the widget, even if the mouse never went down on that widget
- xxxMouseButtonNoLongerDown is called on the widget on which the mouse went down earlier, even if the mouse is no longer on the widget when it goes up
- The textEntered function is meant for handling text input, while keyPressed is meant for special key presses (e.g. backspace or handling CTRL+C events)
- The overriden functions should call the function from the base class (e.g. the implementation of mouseMoved should contain the line `Widget::mouseMoved(pos);`)


### Adding signals

First you simply add the signal object to the class and optionally override the `getSignal` function to support your custom signal:
```c++
tgui::Signal onMyEvent = {"MyEvent"};

tgui::Signal& getSignal(tgui::String signalName) override
{
    if (signalName == onMyEvent.getName())
        return onMyEvent;
    else
        return Widget::getSignal(std::move(signalName));  // Replace "Widget" with the class you inherited from
}
```

When you want the connected callbacks to be executed, you simply call the `emit` function, passing a pointer to your widget as parameter.
```c++
onMyEvent.emit(this);
```

If you want your event to support an optional parameter (e.g. the callback function can have an integer parameter that will be filled in when calling the emit function), then you need to change the signal type from `tgui::Signal` to `tgui::SignalTyped<T>` and pass the parameter to the emit function:
```c++
tgui::SignalTyped<int> onMyEvent = {"MyEvent"};
onMyEvent.emit(this, 5);
```

### Changing renderer

If you want to change the looks of a widget then you aren't required to change the renderer, you can add all functionality to the widget itself (since this is also where the drawing happens). If you want to add renderer properties that are shared between widgets or are present in theme files then you will need to create a custom renderer.

The custom renderer class should inherit from WidgetRenderer, but if your custom widget inherits from e.g. EditBox then the custom renderer can inherit from EditBoxRenderer instead of WidgetRenderer so that you don't need to add all edit box properties yourself.
```c++
class CustomRenderer : public tgui::WidgetRenderer
{
public:
    // Inherit the constructors from the base class
    using tgui::WidgetRenderer::WidgetRenderer;

    // Add whathever functions you want
    void setSomeColor(tgui::Color borderColor);
    tgui::Color getSomeColor() const;
}
```

In the source file for the renderer, you include `RendererDefines.hpp` and use one of the defines that it provides to implement the setter and getter functions.
```c++
#include <TGUI/RendererDefines.hpp>
TGUI_RENDERER_PROPERTY_COLOR(CustomRenderer, SomeColor, {})
```

Now that you have a renderer class, you still need to modify the widget itself to make use of the renderer. First you need to create the renderer in the widget constructor. Note that we are now passing `false` as second parameter to the base class.
```c++
MyCustomWidget(const char* typeName = "MyCustomWidget", bool initRenderer = true) :
    tgui::Widget{typeName, false}
{
    if (initRenderer) // Will always be true unless another class derives from this custom widget
    {
        m_renderer = aurora::makeCopied<CustomRenderer>();
        setRenderer(Theme::getDefault()->getRendererNoThrow(m_type));
    }
}
```

The next thing to do is add the following functions to your custom widget class, to make the `getRenderer()` and `getSharedRenderer()` functions return your custom renderer instead of the one from the base class.
```c++
CustomRenderer* getSharedRenderer()
{
    return aurora::downcast<CustomRenderer*>(Widget::getSharedRenderer());
}

const CustomRenderer* getSharedRenderer() const
{
    return aurora::downcast<const CustomRenderer*>(Widget::getSharedRenderer());
}

CustomRenderer* getRenderer()
{
    return aurora::downcast<CustomRenderer*>(Widget::getRenderer());
}

const CustomRenderer* getRenderer() const
{
    return aurora::downcast<const CustomRenderer*>(Widget::getRenderer());
}
```

Finally you also have to implement the `rendererChanged` function which is called when one of the properties in the renderer is changed.
```c++
void rendererChanged(const tgui::String& property) override
{
    if (property == "SomeColor")
    {
        tgui::Color newValue = getSharedRenderer()->getSomeColor();
    }
    else // Handle the properties from the base class. Replace "Widget" with the class you inherited from.
        Widget::rendererChanged(property);
}
```

### Loading from form file

If you want to be able to save and load your custom widget from a form file then you will first need to register the type with the widget factory (somewhere at the beginning of your program, before trying to load/save the widget).
```c++
tgui::WidgetFactory::setConstructFunction("MyCustomWidget", std::make_shared<MyCustomWidget>);
```

You need to override the save and load functions to add your own properties to the form file. Only properties from the widget itself are needed here, properties in the renderer are automatically saved when they are added as described in the previous section.
```c++
std::unique_ptr<tgui::DataIO::Node> save(SavingRenderersMap& renderers) const override
{
    auto node = Widget::save(renderers);  // Replace "Widget" with the class you inherited from

    node->propertyValuePairs["SomeProperty"] = std::make_unique<tgui::DataIO::ValueNode>("ValueOfTheProperty");

    return node;
}

void load(const std::unique_ptr<tgui::DataIO::Node>& node, const LoadingRenderersMap& renderers) override
{
    Widget::load(node, renderers);  // Replace "Widget" with the class you inherited from

    if (node->propertyValuePairs["SomeProperty"])
    {
        tgui::String value = node->propertyValuePairs["SomeProperty"]->value;
    }
}
```
