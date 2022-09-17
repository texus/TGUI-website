---
layout: page
title: "Example: GLFW_OPENGL3 backend"
breadcrumb: "glfw_opengl3 backend"
---

The code below shows the template that is used for the other examples when using the GLFW\_OPENGL3 backend.

The runExample function is present in all example codes and contains the code that is independent of the backend. The rest of the code depends on GLFW and will thus only be shown here and not repeated in each example.

More information about the minimal code can be found in the [GLFW_OPENGL3 backend tutorial](/tutorials/1.0/backend-glfw-opengl3/).

``` c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/GLFW-OpenGL3.hpp>

// Optional: include OpenGL functions via TGUI (which you can call AFTER creating the Gui object)
// This will include a built-in GLAD header that defines all functions that exist in OpenGL 4.6 (and GLES 3.2)
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

    // The OpenGL renderer backend in TGUI requires at least OpenGL 3.3
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GLFW_TRUE); // Required for macOS

    GLFWwindow* window = glfwCreateWindow(800, 600, "TGUI example - GLFW_OPENGL3 backend", NULL, NULL);
    glfwMakeContextCurrent(window);

    glfwSwapInterval(1);

    run_application(window);

    // All TGUI resources must be destroyed before GLFW is cleaned up
    glfwDestroyWindow(window);
    glfwTerminate();
}
```
