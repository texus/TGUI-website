---
layout: page
title: Texus' Graphical User Interface
redirect_from: "/v0.7-dev/index.html"
---

TGUI is a cross-platform modern c++ GUI library.

Although TGUI was created for SFML, it now also has built-in backends for SDL and GLFW.

<h3>Easy and customizable</h3>
The gui is easy to use, with only a few lines you can e.g. have a fully functional TextBox on your screen. The widgets can be created by just using colors or by using images, making the look very customizable.

<div>
  <a href="/resources/Screenshots/v0.10-White.png" onclick="return showLightBox(event, href);"><img src="/resources/Screenshots/v0.10-White-small.png" alt="TGUI 0.10 White theme" width="246" height="185"/></a>
  <a href="/resources/Screenshots/Black.jpg" onclick="return showLightBox(event, href);"><img src="/resources/Screenshots/Black-small.jpg" alt="TGUI 0.6 Black theme" width="246" height="185"/></a>
  <a href="/resources/Screenshots/BabyBlue.jpg" onclick="return showLightBox(event, href);"><img src="/resources/Screenshots/BabyBlue-small.jpg" alt="TGUI 0.4 BabyBlue theme" width="246" height="185"/></a>
  <a href="/resources/Screenshots/KronosGame.jpg" onclick="return showLightBox(event, href);"><img src="/resources/Screenshots/KronosGame-small.jpg" alt="Kronos Game theme" width="246" height="185"/></a>
</div>

<div>
  <div class="HomePageLargerColumn">
    <h3>Gui Builder</h3>
    <p>TGUI comes with its own Gui Builder, which allows designing your gui more easily. The widgets are loaded in your program by simple calling gui.loadWidgetsFromFile("form.txt").</p>
  </div>
  <div class="HomePageSmallerColumn">
    <a href="/resources/GuiBuilder-0.8.5.png" onclick="return showLightBox(event, href);"><img src="/resources/GuiBuilder-0.8.5-small.jpg" alt="Gui Builder" width="360" height="195" /></a>
  </div>
</div>

<div>
  <div class="HomePageSmallerColumn">
    <img src="/resources/CrossPlatform.jpg" alt="Cross Platform" width="340" height="185" />
  </div>
  <div class="HomePageLargerColumn">
    <h3>Cross-platform</h3>
    <p>TGUI will work on multiple platforms. Not only does it work on <b>Windows</b>, <b>Linux</b> and <b>macOS</b>, but also on <b>Android</b> and <b>iOS</b>. It has also been reported to work on <b>Raspberry Pi</b> and <b>FreeBSD</b>.</p>
  </div>
</div>

<div>
  <div class="HomePageLargerColumn">
    <h3>Modular backends</h3>
    <p>The core of TGUI and its widgets are independent from the rendering, only the backend depends on external libraries.</p>
    <p>Each backend itself is also divided in three reusable components: event input, font loading and rendering. The SFML_OPENGL3 and GLFW_OPENGL3 backends for example share their rendering and font loading code. Custom backends can thus also reuse parts of existing backends.</p>
  </div>
  <div class="HomePageSmallerColumn">
    <a class="light-mode-only" href="/resources/BackendList.png" onclick="return showLightBox(event, href);">
        <img src="/resources/BackendList.png"/>
    </a>
    <a class="dark-mode-only" href="/resources/BackendList-dark.png" onclick="return showLightBox(event, href);">
        <img src="/resources/BackendList-dark.png" class="dark-compatible"/>
    </a>
  </div>
</div>


<!-- Make some of the images use a lightbox when javascript is enabled -->
<script type="text/javascript">
  function showLightBox(event, href) {
    if (event.ctrlKey || event.shiftKey) {
      return true;
    }

    var background = document.createElement("div");
    background.id = "LightBox";
    background.onclick = function() { hideLightBox(); }
    document.getElementById("contents").appendChild(background);

    var image = document.createElement("img");
    image.src = href;
    background.appendChild(image);
    return false;
  }

  function hideLightBox() {
    var lightbox = document.getElementById("LightBox");
    if (lightbox) {
      document.getElementById("contents").removeChild(lightbox);
    }
  }
</script>
