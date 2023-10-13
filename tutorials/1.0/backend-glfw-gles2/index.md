---
layout: page
title: Minimal code with GLFW_GLES2 backend
breadcrumb: minimal code with glfw_gles2
---

The following is the minimal code you need for an application using TGUI with the GLFW_GLES2 backend. Each part of the code and some alternatives are explained below.
{% raw %}
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/GLFW-GLES2.hpp>

#define GLFW_INCLUDE_NONE
#include <GLFW/glfw3.h>

void run_application(GLFWwindow* window)
{
    tgui::Gui gui{window};
    gui.mainLoop();
}

int main()
{
    glfwInit();

    // The OpenGL ES renderer backend in TGUI requires at least OpenGL ES 2.0
    glfwWindowHint(GLFW_CLIENT_API, GLFW_OPENGL_ES_API);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 2);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 0);

    GLFWwindow* window = glfwCreateWindow(800, 600, "TGUI example - GLFW_GLES2 backend", NULL, NULL);
    glfwMakeContextCurrent(window);

    run_application(window);

    // All TGUI resources must be destroyed before GLFW is cleaned up
    glfwDestroyWindow(window);
    glfwTerminate();
}
```
{% endraw %}


### Including TGUI

Since the TGUI library can contain multiple backends, you will always need to include the backend that you want to use. Other than that there is a TGUI.hpp file which includes everything else that you need. This is the easiest way to include TGUI:
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/GLFW-GLES2.hpp>
```

Alternatively, you can selectively include what you need. You will always need `TGUI/Core.hpp` and a backend, but widgets can be included individually:
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backend/GLFW-GLES2.hpp>
#include <TGUI/Widgets/Button.hpp>
#include <TGUI/Widgets/CheckBox.hpp>
```

If you need to use OpenGL ES, you can either let GLFW load the extensions (by removing the `GLFW_INCLUDE_NONE` define), or you can include TGUI's internal code by including `TGUI/Backend/Renderer/OpenGL.hpp`. Note that this header uses GLAD (which gets initialized when creating the Gui object) and defines every function that exists in OpenGL ES 3.2 (so you are responsible for not calling functions newer than the runtime OpenGL ES version).


### Creating the Gui object

Only one gui object should be created for each GLFW window. The gui needs to know which window to render to, so it takes a pointer to the window as parameter to the constructor:
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

Since GLFW works with callbacks, you need to connect all callbacks for events that are relevant to TGUI and pass the event information to the gui. The gui will make sure that the event ends up at the widget that needs it. The following code shows what you need to set up the event callbacks:
```c++
// We set the user pointer so that the callbacks can access the gui
glfwSetWindowUserPointer(window, &gui);

// When we receive an event callback from GLFW, we need to pass the event to TGUI
glfwSetWindowFocusCallback(window, [](GLFWwindow* wnd, int focused) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->windowFocusCallback(focused);
});
glfwSetFramebufferSizeCallback(window, [](GLFWwindow* wnd, int width, int height) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->sizeCallback(width, height);
});
glfwSetCharCallback(window, [](GLFWwindow* wnd, unsigned int codepoint) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->charCallback(codepoint);
});
glfwSetKeyCallback(window, [](GLFWwindow* wnd, int key, int scancode, int action, int mods) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->keyCallback(key, scancode, action, mods);
});
glfwSetScrollCallback(window, [](GLFWwindow* wnd, double xoffset, double yoffset) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->scrollCallback(xoffset, yoffset);
});
glfwSetCursorPosCallback(window, [](GLFWwindow* wnd, double xpos, double ypos) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->cursorPosCallback(xpos, ypos);
});
glfwSetMouseButtonCallback(window, [](GLFWwindow* wnd, int button, int action, int mods) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->mouseButtonCallback(button, action, mods);
});
glfwSetCursorEnterCallback(window, [](GLFWwindow* wnd, int entered) {
    static_cast<tgui::Gui*>(glfwGetWindowUserPointer(wnd))->cursorEnterCallback(entered);
});
```

After the events are configured, you can run your main loop. A minimal main loop would look like this:
```c++
while (!glfwWindowShouldClose(window))
{
    glClear(GL_COLOR_BUFFER_BIT);
    gui.draw();
    glfwSwapBuffers(window);

    glfwWaitEvents();
}
```

The TGUI widgets are drawn with the call to `gui.draw()`. All widgets are drawn at once, if you wish to render OpenGL ES contents inbetween TGUI widgets then you need to use a [Canvas widget](../canvas/) or create a [custom widget](../custom-widgets).
