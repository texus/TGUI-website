---
layout: page
title: DPI awareness and scaling
breadcrumb: dpi scaling
---

### Explanation

Because I only have experience with DPI scaling on Windows, the explanations will focus on Windows. The logic behind it will be similar on other platforms though (if the platform supports such scaling).

A text of 16px might be readable on one screen, but on a screen that has the same physical size but many more pixels (e.g. a 4K monitor), the same 16px text might be too small to read. The size of UI elements should thus depend on the DPI of the monitor. In the Display Settings on Windows you can choose a scale for fonts and UI elements for this reason. This scale factor could be used to scale everything in TGUI to have a readable size on any monitor.

An application can use 3 different DPI awareness modes:  
<table class="with-borders">
  <thead>
    <tr>
      <th></th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Unaware</strong></td>
      <td>The application does nothing, Windows will scale everything for you. Creating an 800x600 window on a monitor with a 200% scale factor will result in a 1600x1200 window on the screen, while the application still thinks it is 800x600. The downside is that everything will look a bit blurry.</td>
    </tr>
    <tr>
      <td><strong>System Aware</strong></td>
      <td>The application is responsible for creating a larger window, Windows will render the window normally. If the window is moved to a monitor with a different scale factor however then Windows will scale the window without the application knowing about it, similar to Unaware.</td>
    </tr>
    <tr>
      <td><strong>Per Monitor</strong></td>
      <td>The application is responsible for creating a larger window and for resizing its window when it is moved between monitors with a different scale factors. This requires at least Windows 8.1 (because older Windows versions didn't have per-monitor scaling), but a V2 can be enabled starting with Windows 10 version 1703 for improved support (e.g. title bar also scales).</td>
    </tr>
  </tbody>
</table>


### Scaling TGUI

Scaling TGUI contents can be done by changing the view of the window. If you want to render everything at 200%, then you simply have to set a view that is half the size as the window, TGUI will then stretches this view to fill the window. By setting a relative view, the size of the view is automatically updated when the window is resized, so that the scale factor remains constant.  
```c++
gui.setRelativeView({0, 0, 0.5f, 0.5f});
```

Scaling the view without other modifications causes text rendering to look bad though. The characters would be rendered at a too low resolution, and additional blurriness may be introduced when not scaling by an integer factor as characters would no longer be pixel-aligned. TGUI provides font scaling to combat this. Setting the font scale will render all characters at a higher resolution internally, and text will be pixel-aligned if the font scale perfectly matches the view scale. Fractional scaling (e.g. 150%) is also supported.  
```c++
tgui::getBackend()->setFontScale(2.0f);
```

Both `setRelativeView` and `setFontScale` would need to called after creating the gui and again whenever the DPI of the window changes (e.g. when the window is dragged to a monitor with a different scaling).

When using a fractional scale (i.e. the scale factor isn't an integer), then sharp rendering for things other than text is not supported by TGUI. At a 150% scale factor, a 1px border would become 1.5px thick. If you have anti-aliasing disabled in the window then some borders will be 1px and others will be 2px thick, leading to a very inconsistent look. It is highly recommended to enable anti-aliasing in the window (as explained below) when using fractional scaling, even though this makes lines look slightly blurry.


### Backend scaling behavior

While TGUI can scale its contents, the actual DPI awareness has to be implemented by the backend and in your code.

Again, this information only describes the behavior on Windows.

#### SFML

SFML 2.6 enables Per Monitor scaling when creating the window, but leaves everything to the user. If you create an 800x600 window, it will be 800x600 on any screen. If the system has a scaling factor of 200% then you are responsible for requesting a 1600x1200 window yourself.

SFML currently doesn't provide a method to retrieve the DPI scaling. SFML also won't change the window size nor send an event when the window switches monitor.

SFML 2.5 mostly works the same, but has broken DPI scaling if you have multiple monitors with a different scale factors. SFML 2.5 enabled System Aware instead of Per Monitor scaling, meaning Windows would stretch the window (similar to Unaware scaling) when the DPI doesn't match the one from the primary monitor.

#### SDL

By default SDL creates a window that is DPI Unaware. Creating an 800x600 window on a monitor with 200% scaling will look like a 1600x1200 window, but function calls to SDL, SDL_Renderer and OpenGL all still think the window is 800x600 (Windows is upscaling the output).

Starting with SDL 2.24.0, this can be changed by calling `SDL_SetHint(SDL_HINT_WINDOWS_DPI_SCALING, "1");` before `SDL_Init`. This will enable Per Monitor scaling or automatically fall back to System Aware on older Windows versions.  
Setting this hint has no effect on platforms other than Windows.

While it is not needed on Windows when setting `SDL_HINT_WINDOWS_DPI_SCALING`, you can pass `SDL_WINDOW_ALLOW_HIGHDPI` as flag to `SDL_CreateWindow` to have high-dpi support on other platforms (e.g. macOS and iOS).

With scaling enabled, passing 800x600 to `SDL_CreateWindow` will also result in a 1600x1200 window (on a monitor with 200% scaling), but this time you can render to all the pixels yourself as Windows is no longer upscaling the rendering.
Note that `SDL_GetWindowSize` will still return 800x600, you have to use `SDL_GetWindowSizeInPixels` to retrieve the real window size (or use `SDL_GetRendererOutputSize` or `SDL_GL_GetDrawableSize` to support SDL < 2.26). Mouse events are also still mapped to the 800x600 size.

When moving the window to a monitor with different scaling, the window size in Windows changes. If DPI scaling was enabled then the drawable area also changes and an `SDL_WINDOWEVENT_SIZE_CHANGED` event is send to inform you about it.

#### GLFW

GLFW enables Per Monitor scaling when available, falling back to System Aware for older Windows versions. The exact behavior of the window size depends on the GLFW version though. 

GLFW 3.2:
- If you create an 800x600 window, you will get an 800x600 window instead of it being scaled. You are responsible for requesting a 1600x1200 window yourself if the system has a scaling factor of 200%.
- When moving the window to a monitor with a different scale factor, the window is resized according to the scale factor (and you receive WindowSize and FramebufferSize callbacks). The framebuffer and window sizes thus stay in sync.

GLFW 3.3 default:
- If you create an 800x600 window, you will get an 800x600 window instead of it being scaled.
- When moving the window to a monitor with a different scale factor, the window keeps it 800x600 size.

GLFW 3.3 with GLFW\_SCALE\_TO\_MONITOR:
- Call `glfwWindowHint(GLFW_SCALE_TO_MONITOR, 1)` before creating the window (but AFTER calling `glfwInit()`).
- Passing 800x600 to `glfwCreateWindow` will result in a 1600x1200 window (on a monitor with 200% scaling), with both `glfwGetWindowSize` and `glfwGetFramebufferSize` reporting this higher size.
- When moving the window to a monitor with a different scale factor, the window is resized according to the scale factor (and you receive WindowSize and FramebufferSize callbacks). The framebuffer and window sizes thus stay in sync.

Since GLFW 3.3 you can use `glfwGetWindowContentScale` to retrieve the scale factor of the monitor on which the window is located, and you can use `glfwSetWindowContentScaleCallback` to also receive an event when the window is dragged to a monitor with a different scale factor.


### Enabling anti-aliasing

Anti-aliasing is not required when the scale factor is a multiple of 100% or for rendering text when setting the correct font scale. When using a fractional scale factor however, all lines and rectangles rendered by TGUI would no longer correctly map to pixels (because they would have a thickness of e.g. 1.5px). With anti-aliasing disabled, rendering will be rounded to the nearest pixel which causes some lines to be thicker than others, leading to a very inconsistent look. By enabling anti-aliasing, the line will appear slightly blurred, but the lines would at least look like they have the same thickness.

Anti-aliasing is a property of the window and the way to enable it thus depends on the backend.

#### SFML

The sampling level can be specified in the ContextSettings that can be provided at window creation:  
```c++
sf::ContextSettings settings;
settings.antialiasingLevel = 4;
sf::RenderWindow window(sf::VideoMode{800, 600}, "Title", sf::Style::Default, settings);
```

#### SDL

When using a backend with OpenGL (or GLES), add the following between the `SDL_Init` call and creating the window to enable 4x multisampling:  
```c++
SDL_GL_SetAttribute(SDL_GL_MULTISAMPLEBUFFERS, 1);
SDL_GL_SetAttribute(SDL_GL_MULTISAMPLESAMPLES, 4);
```

The above code also works with the `SDL_RENDERER` backend, as long as the SDL_Renderer class is internally using OpenGL for rendering.

#### GLFW

Between the `glfwInit()` call and creating the window, add the following line to enable 4x MSAA:  
```c++
glfwWindowHint(GLFW_SAMPLES, 4);
```
