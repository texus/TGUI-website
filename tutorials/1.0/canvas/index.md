---
layout: page
title: Canvas widget
breadcrumb: canvas
---

When calling `gui.draw()`, all (visible) widgets are drawn at once. If you wish to manually render something inbetween TGUI widgets (e.g. to the background of a child window) then you can either create a custom widget or simply make use of one of the Canvas widgets. The canvas acts as a render target: you first render your contents to the canvas and then when TGUI draws its widgets, it will draw the contents of the canvas widget to the screen.

If the contents is static then the canvas can be rendered to only once, you don't have to redraw it each frame. Note however that resizing the canvas widget will leave its contents in an undefined state, so you must redraw its contents if you resize the widget.

The canvas widget is specific to the backend renderer. Its header file is automatically included when including the backend.

### CanvasSFML

The CanvasSFML widget is only available when using the SFML\_GRAPHICS backend.

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasSFML::create({400, 300});
gui.add(canvas);
```

Before rendering, you should always clear the contents:
```c++
canvas->clear(tgui::Color::Black);
```

The canvas provides the same draw functions as sf::RenderTarget, so you can call `draw` to render SFML objects:
```c++
sf::Text text;
sf::Sprite sprite;
canvas->draw(text);
canvas->draw(sprite);
```

After rendering, you must always call the `display()` function to indicate that rendering has finished:
```c++
canvas->display();
```

### CanvasSDL

The CanvasSDL widget is only available when using the SDL\_RENDERER backend.

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasSDL::create({400, 300});
gui.add(canvas);
```

Before rendering, you must tell SDL that further commands should render to the canvas instead of the window:
```c++
SDL_SetRenderTarget(renderer, canvas->getTextureTarget());
```

Afterwards you can draw just like you would normally draw on the window:
```c++
SDL_Texture* imgTexture;
SDL_RenderClear(renderer);
SDL_RenderCopy(renderer, imgTexture, NULL, NULL);
```

After rendering, you must always tell SDL to stop rendering on the canvas:
```c++
SDL_SetRenderTarget(renderer, nullptr);
```


### CanvasSDLGPU

The CanvasSDLGPU widget is only available when using the SDL\_GPU backend.

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasSDLGPU::create({400, 300});
gui.add(canvas);
```

Rendering to the canvas is done inside an SDL\_GPURenderPass for which the texture in SDL\_GPUColorTargetInfo was set to the texture of the canvas. Afterwards you can draw just like you would normally draw on the window.
```c++
SDL_GPUColorTargetInfo colorTargetInfoCanvas;
colorTargetInfoCanvas.texture = canvas->getTexture();
colorTargetInfoCanvas.clear_color = clearColor;
colorTargetInfoCanvas.load_op = SDL_GPU_LOADOP_CLEAR;
colorTargetInfoCanvas.store_op = SDL_GPU_STOREOP_STORE;
SDL_GPURenderPass* renderPass = SDL_BeginGPURenderPass(cmdBuffer, &colorTargetInfoCanvas, 1, NULL);
```

### CanvasOpenGL3

The CanvasOpenGL3 widget is only available when using a backend that uses the OpenGL3 renderer (i.e. SFML\_OPENGL3, SDL\_OPENGL3, SDL\_TTF\_OPENGL3 or GLFW\_OPENGL3 backends).

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasOpenGL3::create({400, 300});
gui.add(canvas);
```

Before rendering, you must tell OpenGL that further commands should render to the canvas instead of the window.
```c++
canvas->bindFramebuffer();
```

You may also want to call `glViewport(0, 0, canvas->getSize().x, canvas->getSize().y);` to scale rendering to the canvas size instead of the window size.

Afterwards you can draw just like you would normally draw on the window:
```c++
glClear(GL_COLOR_BUFFER_BIT);
```

After rendering, you must always tell OpenGL to stop rendering on the canvas:
```c++
glBindFramebuffer(GL_FRAMEBUFFER, 0);
```

### CanvasGLES2

The CanvasGLES2 widget is only available when using a backend that uses the GLES2 renderer (i.e. SFML\_GLES2, SDL\_GLES2, SDL\_TTF\_GLES2 or GLFW\_GLES2 backends).

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasGLES2::create({400, 300});
gui.add(canvas);
```

Before rendering, you must tell OpenGL ES that further commands should render to the canvas instead of the window.
```c++
canvas->bindFramebuffer();
```

You may also want to call `glViewport(0, 0, canvas->getSize().x, canvas->getSize().y);` to scale rendering to the canvas size instead of the window size.

Afterwards you can draw just like you would normally draw on the window:
```c++
glClear(GL_COLOR_BUFFER_BIT);
```

After rendering, you must always tell OpenGL ES to stop rendering on the canvas:
```c++
glBindFramebuffer(GL_FRAMEBUFFER, 0);
```

### CanvasRaylib

The CanvasRaylib widget is only available when using the RAYLIB backend.

Creating the canvas is like creating any other widget. It's size can be provided to the create function for convenience:
```c++
auto canvas = tgui::CanvasRaylib::create({400, 300});
gui.add(canvas);
```

Before rendering, you must tell raylib that further commands should render to the canvas instead of the window:
```c++
BeginTextureMode(canvas->getTextureTarget());
```

Afterwards you can draw just like you would normally draw on the window:
```c++
ClearBackground(RAYWHITE);
DrawRectangle(0, 0, 200, 100, YELLOW);
```

After rendering, you must always tell raylib to stop rendering on the canvas:
```c++
EndTextureMode();
```
