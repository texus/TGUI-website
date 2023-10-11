---
layout: page
title: Signals (event callbacks)
breadcrumb: signals
---

### Connecting to signals
When something happens, widgets can send out a signal (e.g. a button has a Press signal and a list box has an ItemSelect signal). The signals are public members of the widgets (starting with the "on" prefix, e.g. "onPress").

Here is an example of having a function called when a button has been pressed.
```c++
void callbackFunc()
{
    std::cout << "Button pressed" << std::endl;
}

button->onPress(&callbackFunc);
```

You could also use lambda functions of course if your function is very small. This is all the code you need to close the window when your quit button gets pressed:
```c++
quitButton->onPress([&]{ window.close(); });
```


### Custom parameters

Custom parameters can also be provided. They have to be provided when connecting the callback function and will be passed to the callback function when the event occurs. Keep in mind that when passing a variable, a copy will be made. If the signal handler needs to have access to the original variable, use a pointer or pass the reference with std::ref.
```c++
void signalHandler1(int i, float f);
editBox->onTextChange(&signalHandler1, 5, 1.2f);

void signalHandler2(tgui::Gui& gui);
editBox->onTextChange(&signalHandler2, std::ref(gui));
```


### Class member functions

When connecting member functions, you must also keep in mind that the first parameter of such function is a hidden 'this' pointer. You must always provide the value of the 'this' pointer manually.
```c++
class Class
{
public:
    void signalHandler1();
    void signalHandler2(tgui::Gui& gui);

private:
    tgui::CheckBox::Ptr m_checkBox = tgui::CheckBox::create();

    void f()
    {
        m_checkBox->onChange(&Class::signalHandler1, this);
        m_checkBox->onChange(&Class::signalHandler2, this, std::ref(gui));
    }
};

Class instance;
picture->onClick(&Class::signalHandler1, &instance);
picture->onClick(&Class::signalHandler2, &instance, std::ref(gui));
```


### Optional parameters

Widgets can optionally provide information about the event that occurred. For example, the onPress signal from buttons provides the text of the button as optional parameter, so that the signal handler can distinguish which button was pressed.
```c++
void buttonPressedCallback1();
button1->onPress(&buttonPressedCallback1);

void buttonPressedCallback2(tgui::String buttonText);
button2->onPress(&buttonPressedCallback2);

void buttonPressedCallback3(const tgui::String& buttonText);
button3->onPress(&buttonPressedCallback3);
```

When combined with custom parameters, the custom parameters have to be the first parameters of the function (i.e. the `int` and `tgui::String` parameters can't be in reverse order).
```c++
button4->onPress([](int customValue, tgui::String buttonText){ /* ... */ }, 5);
```


### Disconnecting signals

So far the signal has been treated as a callable function. It is actually a functor object where the call operator is just a shortcut for the "connect" function. Other functions also exist, such as the "disconnect" function.

A unique id is returned when connecting a signal, which can be used to diconnect the callback function later:
```c++
unsigned int id = slider->onValueChange(signalHandler);
slider->onValueChange.disconnect(id);
```


### Temporarily ignoring signals

When e.g. calling `checkBox->setChecked(true)`, the `onChange` signal would be emitted if the check box wasn't already checked. If you only want to receive the callback when the user changes the state and don't want to receive a callback for this manual change in the code then you can temporarily disable the signal:
```c++
checkBox->onChange.setEnabled(false);
checkBox->setChecked(true);
checkBox->onChange.setEnabled(true);
```


### Accessing signal by name

If you don't know the type of the widget, or the signal to connect is dynamically loaded (e.g. a text file specifying the name of the signal to connect), then you can't access the signal member directly. So there is an alternative method to access these signals by just specifying their name as a string:
```c++
widget->getSignal("TextChanged").connect([]{ std::cout << "Text was changed\n"; });
```

Note that when using this method, the optional parameters are not supported (as getSignal returns the base signal type instead of knowing the exact type). While the signal object is called onTextChange, the name is "TextChanged" (past tense). If you are uncertain about the string to use, you can find it by accessing `editBox->onTextChange.getName()`.

If you intent to create a single callback function for all signals then you can make use of the `connectEx` function. The callback function passed to this function has 2 parameters: the widget that triggers the signal and the name of the signal.
```c++
void callbackFunc(Widget::Ptr widget, const String& signalName);
widget->getSignal(signalName).connectEx(&callbackFunc);
```


### Lambda captures

If you are relatively new to lambdas then you might not fully understand when captured variables are copied or accessed by reference, so this section provides some more information about capturing by value or by reference.

In example code you will often see the lambda start with either `[]`, `[=]` or `[&]`. There is a big difference between them.
- With `[]`, the lambda is simply an inline function. It can't access any of the variables outside the lambda.
- With `[=]`, the lambda has access to variables outside the lambda body, but all values are copies and you thus can't change them.
- With `[&]`, the lambda has access to variables outside the lambda body, by reference. No copies are made and variables can be changed. This should only be used when you actually need it as it may introduce crashes when used incorrectly as explained further down below.

In a more advanced situation you can combine capture by value and capture by reference by specifying the individual variables (with or without an ampersand in front of the name to choose by reference vs. by value). All three functions below are identical.
```c++
int x, y;
auto func1 = [=,&y]{ y = x; };  // x is copied due to "=", y is reference due to "&y"
auto func2 = [&,x] { y = x; };  // x is copied due to "x", y is reference due to "&"
auto func3 = [x,&y]{ y = x; };  // x is copied due to "x", y is reference due to "&y"
```

The gui object can't be copied, it must always be passed as a reference (but if you store a pointer to it then you can obviously pass that pointer by value):
```c++
tgui::Gui gui;
button1->onPress([&]{ gui.setOpacity(0.5f); });
```

Note that objects such as `Button::Ptr` are in fact pointers. Because of this, passing widgets to a lambda simply copies the pointer and not the widget itself. This is why it is fine to pass widgets by value:
```c++
button->onPress([=]{ button->setText("Hello"); });
```

To understand why passing a variable by reference is sometimes undesired or dangerous, it is important to understand when your lambda function is executed. When you connect your lambda to the signal, the code in the lambda isn't executed yet. The execution happens later when the callback occurs (e.g. when the button is pressed). If your referenced parameter no longer exists at that time, you will get undefined behavior (usually a crash). Take the following example:
```c++
for (int i = 0; i < 10; ++i)
{
    auto button = createButton(i);
    button->onPress([&]{ cout << i << endl; }); // This is BAD!
}
```

The above code stores a reference to the local variable `i`. When the button is pressed, this variable has already gone out of scope so your program will either crash or print garbage. The undefined behavior could be fixed by declaring `i` in global scope instead of inside the for loop, but in this case the value `10` will be printed no matter on which button you clicked (because that is the value of `i` after the for loop finished). You have to change `[&]` into `[=]` if you want the index of the button to be printed when it is pressed. An alternative solution would be to pass the value as custom parameter to the function instead of using the lambda capture to copy it (i.e. `button->onPress([](int x){ cout << x << endl; }, i)`).
