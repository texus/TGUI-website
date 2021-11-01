---
layout: page
title: "Example: GLFW_GLES2 backend"
breadcrumb: "glfw_gles2 backend"
---

The code below shows the template that is used for the other examples when using the GLFW\_GLES2 backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on GLFW and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [GLFW_GLES2 backend tutorial](/tutorials/0.10/backend-glfw-gles2/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/GLFW-GLES2.hpp>

// Optional: include OpenGL ES functions via TGUI (which you can call AFTER creating the Gui object)
// This will include a built-in GLAD header that defines all functions that exist in GLES 3.2 (and OpenGL 4.6)
//#include <TGUI/Backend/Renderer/OpenGL.hpp>

#define GLFW_INCLUDE_NONE // Don't let GLFW include an OpenGL extention loader
#include <GLFW/glfw3.h>

bool runExample(tgui::BackendGui& gui)
{
    return true;
}

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying GLFW
void run_application(GLFWwindow* window)
{
    tgui::Gui gui{window};
    if (!runExample(gui))
        return;

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

    glfwSwapInterval(1);

    run_application(window);

    // All TGUI resources must be destroyed before GLFW is cleaned up
    glfwDestroyWindow(window);
    glfwTerminate();
}
```
