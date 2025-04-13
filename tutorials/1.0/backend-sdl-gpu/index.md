---
layout: page
title: Minimal code with SDL_GPU backend
breadcrumb: minimal code with sdl_gpu
---

The following is the minimal code you need for an application using TGUI with the SDL\_GPU backend. The code contains some comments with explanations, but some extra information and some alternatives are also provided below the example code.
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SDL-GPU.hpp>

// We don't put this code in main() to make sure that all TGUI resources are destroyed before destroying SDL
void run_application(SDL_Window* window, SDL_GPUDevice* device)
{
    tgui::Gui gui(window, device);
    gui.mainLoop(); // See below for how to use your own main loop
}

// Note that no error checking is performed on SDL initialization in this example code
int main(int, char **)
{
    SDL_Init(SDL_INIT_VIDEO);

    SDL_GPUDevice* device = SDL_CreateGPUDevice(SDL_GPU_SHADERFORMAT_SPIRV | SDL_GPU_SHADERFORMAT_DXIL | SDL_GPU_SHADERFORMAT_MSL, false, nullptr);
    SDL_Window* window = SDL_CreateWindow("TGUI example (SDL-GPU)", 800, 600, SDL_WINDOW_RESIZABLE);
    SDL_ClaimWindowForGPUDevice(device, window);

    // SDL_ttf needs to be initialized before using TGUI
    TTF_Init();

    run_application(window, device);

    // Note that all TGUI resources must be destroyed before SDL_ttf is cleaned up
    TTF_Quit();

    SDL_ReleaseWindowFromGPUDevice(device, window);
	SDL_DestroyWindow(window);
	SDL_DestroyGPUDevice(device);
    SDL_Quit();
    return 0;
}
```


### Including TGUI

Since the TGUI library can contain multiple backends, you will always need to include the backend that you want to use. Other than that there is a TGUI.hpp file which includes everything else that you need. This is the easiest way to include TGUI:
```c++
#include <TGUI/TGUI.hpp>
#include <TGUI/Backend/SDL-GPU.hpp>
```

Alternatively, you can selectively include what you need. You will always need `TGUI/Core.hpp` and a backend, but widgets can be included individually:
```c++
#include <TGUI/Core.hpp>
#include <TGUI/Backend/SDL-GPU.hpp>
#include <TGUI/Widgets/Button.hpp>
#include <TGUI/Widgets/CheckBox.hpp>
```


### Creating the gui object

Only one gui object should be created for each SDL window. The gui needs to know which window to render to, so it takes a pointer to the window and the associated SDL_GPUDevice as parameters to the constructor:
```c++
tgui::Gui gui{window, device};
```

The gui also has a default constructor. If you use it then you will however need to call setWindow before interacting with the gui object.
```c++
tgui::Gui gui;
gui.setWindow(window, device);
```

Note that the Gui object is used for global initialization and destruction in TGUI. **You MUST NOT create other TGUI objects before the Gui has been given a window and you MUST NOT use TGUI objects after the last Gui object is destroyed**.


### Main loop

Although TGUI provides a `gui.mainLoop()` function for convenience, in many cases you will need to use your own main loop (e.g. if you need to draw things other than the gui or run code that isn't event-based).

A minimal main loop would look like this:
```c++
bool quit = false;
while (!quit)
{
    SDL_Event event;
    while (SDL_PollEvent(&event) != 0)
    {
        gui.handleEvent(event);

        if (event.type == SDL_EVENT_QUIT)
            quit = true;
    }

    SDL_GPUCommandBuffer* cmdBuffer = SDL_AcquireGPUCommandBuffer(device);

    SDL_GPUTexture* swapchainTexture = nullptr;
    SDL_WaitAndAcquireGPUSwapchainTexture(cmdBuffer, window, &swapchainTexture, NULL, NULL);
    if (swapchainTexture)
    {
        SDL_GPUCopyPass* copyPass = SDL_BeginGPUCopyPass(cmdBuffer);
        gui.prepareDraw(cmdBuffer, copyPass);
        SDL_EndGPUCopyPass(copyPass);

        SDL_GPUColorTargetInfo colorTargetInfo = {};
        colorTargetInfo.texture = swapchainTexture;
        colorTargetInfo.clear_color = {200.f / 255.f, 200.f / 255.f, 200.f / 255.f, 1.f};
        colorTargetInfo.load_op = SDL_GPU_LOADOP_CLEAR;
        colorTargetInfo.store_op = SDL_GPU_STOREOP_STORE;
        SDL_GPURenderPass* renderPass = SDL_BeginGPURenderPass(cmdBuffer, &colorTargetInfo, 1, NULL);
        gui.draw(renderPass);
        SDL_EndGPURenderPass(renderPass);
    }

    SDL_SubmitGPUCommandBuffer(cmdBuffer);
}
```

In your event loop, `gui.handleEvent(event)` is used to inform the gui about the event. The gui will make sure that the event ends up at the widget that needs it. If all widgets ignored the event then `handleEvent` will return `false`. This could be used to e.g. check if a mouse event was handled by the gui or should still be handled by your own code.

To draw all widgets in the gui, you need to call both `gui.prepareDraw` and `gui.draw` once per frame. All widgets are drawn at once, if you wish to render SDL contents inbetween TGUI widgets then you need to use a [Canvas widget](../canvas/) or create a [custom widget](../custom-widgets).
