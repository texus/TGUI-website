---
layout: page
title: Changelog

changelog:
- version: 1
  minors:
  - version: 4
    patches:
    - version: 1
      date: 20 July 2024
      changes: |
        Fixed infinite loop in Theme::replace (introduced in TGUI 1.4.0)
        PanelListBox now has proper background color and borders in White theme
    - version: 0
      date: 15 July 2024
      changes: |
        New widget: SplitContainer
        Added MaxValue getter to Scrollbar
        Added ScrollbarMaxValue getters to widgets with a scrollbar
        Added getPixelsPerPoint() to BackendRenderTarget
        Inner size of ScrollablePanel now depends on shown scrollbars
        Replaced VerticalScroll with Orientation in Slider, Scrollbar and SpinButton
        Multiple fixes to EditBoxSlider widget
  - version: 3
    patches:
    - version: 0
      date: 10 June 2024
      changes: |
        New backend: raylib
        New widget: EditBoxSlider ([PR #238](https://github.com/texus/TGUI/pull/238))
        All widgets can now be configured to ignore mouse events
        Added HorizontalLayout and VerticalLayout to replace widget-specific enums
        Added method to associate user data to combo box items
        Added onWindowFocus and onWindowUnfocus signals
        Renamed isKeptInParent to getKeepInParent in ChildWindow
        Renamed limitTextWidth to setTextWidthLimited in EditBox
        String::fromNumber now supports int8_t (but no longer accepts pointers)
        BackendTextureSFML::getInternalTexture() now returns a pointer
        BackendFontSFML::getInternalFont() now return a pointer
  - version: 2
    patches:
    - version: 0
      date: 23 March 2024
      changes: |
        Added Theme::replace function
        Added TreeView::changeItem function
        Added TreeView::getNode function
        Added ignoreMouseEvents function to canvas widgets
        Added Panel::setEventBubbling to enable more intuitive event propagation
        Replaced getWidgetAtPosition with getWidgetAtPos
        getWidgetBelowMouseCursor was given a parameter for recursive search
        Textures with different part rects were incorrectly considered equal
        showWithEffect didn't show widget if a hide animation was still playing
        Setting opacity of a SubWidgetContainer didn't work
        SubWidgetContainer didn't support show/hide animations
  - version: 1
    patches:
    - version: 0
      date: 4 November 2023
      changes: |
        Added AutoLayout that lets widget fill entire side of parent
        Any column in ListView can now be auto-sized and expanded
        Added methods for arrow key navigation between widgets
        Added getColumnDesignWidth function to ListView
        Added TextOutlineColor and TextOutlineThickness to ProgressBar renderer
        MiddleRect of Texture can now be changed after loading
        Hover state is now reset when mouse leaves the window
  - version: 0
    patches:
    - version: 0
      date: 30 September 2023
      changes: |
        Added PanelListBox widget ([PR #193](https://github.com/texus/TGUI/pull/193))
        FileDialog can now create new folders ([PR #192](https://github.com/texus/TGUI/pull/192))
        Added MessageBox::changeButtons to set multiple buttons at once ([PR #215](https://github.com/texus/TGUI/pull/215))
        Added methods to ScrollablePanel to check if scrollbar is currently shown ([PR #213](https://github.com/texus/TGUI/pull/213))
        Pressing the tab key can now insert custom text in TextArea ([PR #211](https://github.com/texus/TGUI/pull/211))
        Widgets in SubwidgetContainer didn't inherit the font of the container ([PR #208](https://github.com/texus/TGUI/pull/208))
        Added onCaretPositionChange, getCaretLine() and getCaretColumn() to TextArea ([PR #207](https://github.com/texus/TGUI/pull/207))
        Added onCaretPositionChange signal to EditBox ([PR #206](https://github.com/texus/TGUI/pull/206))
        Added missing getSignal() functions to TabContainer and SpinControl ([PR #204](https://github.com/texus/TGUI/pull/204))
        BoxLayout::setWidgetIndex didn't immediately update the positions ([PR #203](https://github.com/texus/TGUI/pull/203))
        Added tab alignment and fixed tab size to TabContainer ([PR #174](https://github.com/texus/TGUI/pull/174))
        Added SDL\_Renderer, GLFW/OpenGL and SFML/OpenGL backends
        Added RichTextLabel widget
        Support two finger scrolling on touch screens
        Added font scaling to keep text sharp while view is smaller than window size
        Textures can now be loaded from base64 string
        ListView columns can now be resizable
        ListView icons can also be saved in form file
        Position of text in buttons can now be changed
        Added changeMenuItem function to MenuBar to change the text of a menu
        Added SizeHorizontal and SizeVertical mouse cursors
        Added hasUserData to Widget
        Added LabelAlignment and ButtonAlignment to MessageBox
        Added ScrollbarValue to Label
        Theme files now support global properties
        Theme files now support inheritance between sections
        Improved scrolling with nested scrollbars
        Added UseWideArrows property to SpinControl
        Added moveWithAnimation and resizeWithAnimation functions to Widget
        Added case-insensitive variants of startsWith and endsWith to String
        Tool tips are now shown on disabled widgets by default
        Word-wrapped lines no longer begin with whitespace
        IME pre-edit window will be positioned next to the text cursor on Windows
        Typing in FileDialog now selects the first file starting with the typed letter
        Added getWindow() function to Gui
        handleEvent now always returns true for scroll events when mouse is on top of a widget
        onFileSelect signal in FileDialog is no longer called on cancel
        Renamed onSelectionChanged to onSelectionChange in TabContainer
        TabContainer now inherits from Container instead of SubwidgetsContainer
        Removed padding from RadioButtonGroup
        Black, BabyBlue and TransparentGrey themes can now be used for all widgets
        ClientSize of ChildWindow can now be a layout instead of only a constant
        TextSize can now also be set in theme file
        Many bug fixes and minor improvements
- version: 0
  minors:
  - version: 9
    patches:
    - version: 5
      date: 17 July 2022
      changes: |
        Fixed Gui Builder crash when creating or loading any form
        Fixed crash when position of ChildWindow depends on its size
    - version: 4
      date: 15 June 2022
      changes: |
        Default theme no longer affects loading widgets from file
        Added TextAlignment property to ListBox
        Added onScroll signal to ListBox
        Added option to show tool tips on disabled widgets
        Up and down keys in ListBox now move scrollbar if needed
        Renderers shared in form files no longer save in random order
        Pasting still worked in TextArea when it was read-only
        ListBox::setMaximumItems didn't reset selected item when removed
        Fixed issue with renderer property within custom renderer outside tgui namespace
        Parsing invalid UTF-8 character could trigger a debug assert in Visual Studio
        Widget could remain in hover state after hiding it
        Fixed broken text rendering in SDL/OpenGL backend
        List of ComboBox did not use the font set in the ComboBox
        Keeping child window inside parent didn't take origin into account
    - version: 3
      date: 15 January 2022
      changes: |
        Added RoundedBorderRadius property to Panel renderer
        Touch events didn't work in SDL backend
        Default DistanceToSide in Tabs is now rounded to nearest integer
        Adding widget to container no longer overwrites name if no name is given
        Container::get now also searches inside SubwidgetContainer widgets
        Renamed onAnimationFinish to onShowEffectFinish
        Renamed ShowAnimationType to ShowEffectType
        Loading BMP files didn't work
        Scrollbar in Label didn't inherit opacity
        showWithEffect/hideWithEffect didn't work properly is position was percentage
    - version: 2
      date: 1 November 2021
      changes: |
        Arrow keys can now be used to navigate TreeView
        Control/shift now affect using arrow keys in multi-select ListView
        Added Texture::setDefaultSmooth to choose default interpolation setting
        String::split now takes an optional boolean to trim the returned values
        Kerning calculation now takes bold text style into account (only if SFML >= 2.6)
        Added setWidgetIndex and getWidgetIndex functions to container
        Added String::contains function
        saveWidgetsToFile now makes paths relative to the form path
        Tool tips will no longer be displayed outside the window when close to the side
        Exception is now thrown when loading a font fails
        TabContainer can now create the panels internally
        Button text was lost when copying button
        Down state of button wasn't displayed properly anymore when a focus state existed
        Image in BitmapButton was searched in wrong directory when loading from form file
        Form files didn't load on Windows when path contained non-ANSI characters
        Modification time in FileDialog was empty when MinGW compiler was used
        Clipping in SDL backend was incorrect when view and viewport were changed
        Opacity of color wheel wasn't set when making ColorPicker transparent
    - version: 1
      date: 12 February 2021
      changes: |
        Added new SpinControl widget (combination of EditBox and SpinButton) ([PR #135](https://github.com/texus/TGUI/pull/135))
        Added new TabContainer widget (combination of Tabs with Panel below) ([PR #139](https://github.com/texus/TGUI/pull/139))
        Holding down arrow on SpinButton will now keep chaning the value ([PR #137](https://github.com/texus/TGUI/pull/137))
        Added insertItem function to ListView ([PR #138](https://github.com/texus/TGUI/pull/138))
        Up and down arrows now change selected item in ListBox and ListView ([PR #146](https://github.com/texus/TGUI/pull/146))
        Horizontal scrollbar can now depend on item width in ListView ([PR #147](https://github.com/texus/TGUI/pull/147))
        Added support to copy selected ListView items to clipboard ([PR #148](https://github.com/texus/TGUI/pull/148))
        Add setTextSize for SubwidgetContainer ([PR #149](https://github.com/texus/TGUI/pull/149))
        Added SDL/OpenGL backend as alternative for default SFML backend
        Rewrote signal system again, `b->connect("Pressed",...)` is now `b->onPress(...)`
        Added new FileDialog widget
        Added new ToggleButton widget
        Added new SeparatorLine widget
        Added support for setting mouse cursor + use them on resizable child windows
        Added timers and optional gui.mainLoop()
        Added experimental setOrigin, setScale and setRotation functions to Widget
        Added ThumbWithinTrack to Slider renderer to have thumb align with track on sides
        Added DoubleClick signal to Panel
        Added ViewChanged signal to GuiBase
        Added hover and selected border colors for Tabs
        Added TextureSelectedTrack property to RangeSlider renderer
        Added String::fromNumberRounded to convert float to string with a fixed amount of decimals
        Added startsWith and endsWith helper functions to String
        Added RoundedBorderRadius property to button renderer
        Separators can be added to MenuBar by inserting menu items with "-" string
        ListBox and ListView can now store user data in their items
        ListView icons can be given a fixed size to rescale all icons to requested size
        Replaced all std::string and sf::String by tgui::String
        Replaced Text, Color, Rect and Vector2 classes from SFML with own versions
        Replaced sf::Text::Style with tgui::TextStyle
        Replaced setView in Gui by view/viewport setters with absolute or relative values
        Added onClosing signal to ChildWindow to have a better way to abort closing it
        Signal names, renderer names and widget/renderer properties are now case-sensitive
        Added setWidth and setHeight helper functions
        Added getCheckedRadioButton to RadioButtonGroup
        Textures are now smooth by default
        Size of ChildWindow now includes borders and title bar
        When setView is not called on Gui, view will now scale with window size
        Menus of menu bar are now always on top of all other widgets
        Swapped padding and alignment parameters of addWidget in Grid
        Moved uncheckRadioButtons from Container to RadioButtonGroup
        Theme::setDefault now takes a shared_ptr or a filename as parameter
        Changed return type of addItem in ListBox/ComboBox to return index of the item
        Changed Knob value type from int to float
        Changed default value of ChangeItemOnScroll in ComboBox to false
        Changed default value of ExpandDirection in ComboBox to Automatic
        Changed default value of ScrollbarPolicy in Label to Automatic
        Renamed TextBox to TextArea
        Renamed getMenuList in MenuBar to getMenus
        Renamed mouseOnWidget to isMouseOnWidget
        Renamed TextStyle class to TextStyles and TextStyles::Style enum to TextStyle
        Default scroll amount in ScrollablePanel now depends on global text size
        Gui::getFont now returns the global font if no font was set in the Gui
        Container now translates the widget position before calling draw function
        Dragging scrollbar inside child window didn't work when mouse left child window
        Selected part of RangeSlider wasn't drawn when using textures
        Removed all code that was marked as deprecated
        Binding left and top in layouts now works correctly when the origin is changed
        Some other small changes that weren't added to the changelog
  - version: 8
    patches:
    - version: 9
      date: 12 February 2021
      changes: |
        ColorPicker widget added ([PR #145](https://github.com/texus/TGUI/pull/145))
        Added sortWidgets to Container to update the z-order of all widgets ([PR #141](https://github.com/texus/TGUI/pull/141))
        Added getWidgetBelowMouseCursor to Gui and getWidgetAtPosition to Container/Gui
        Added moveWidgetForward/moveWidgetBackward to Container to move widget one step in z-order
        Added parameter to focusNextWidget/focusPreviousWidget in Container to not look recursively
        Added enableSkipDrawingWidgetsOutsideView() to ScrollablePanel to enable optimization
        Tabs::remove didn't recalculate the tabs width
        SubwidgetContainer wasn't being compiled into the library
        Text orientation in Button wasn't updated when size changed
        Text position in EditBox didn't update when increasing width if text didn't fit before
    - version: 8
      date: 20 June 2020
      changes: |
        Added RightClicked signal to TreeView ([PR #125](https://github.com/texus/TGUI/pull/125))
        Changed the keyboard shortcuts in EditBox and TextBox for macOS
        Added getIndexById and getIdByIndex to ListBox
        Added global setEditCursorBlinkRate function
        Added renderer properties in ComboBox for disabled state
        Added bind functions to layouts to bind to inner size
        Added getFocusedChild() and getFocusedLeaf() to Container and Gui
        Made updateTime() in Gui public and let it return whether something changed
        Picture had wrong size when loading from file with a relative size
        String to float conversion could fail since 0.8.6 when C locale was changed
    - version: 7
      date: 8 February 2020
      changes: |
        TextBox can now have a default text that is displayed when it is empty ([PR #117](https://github.com/texus/TGUI/pull/117))
        Added SignalManager class to connect signals by widget name (even if widget not loaded yet) ([PR #112](https://github.com/texus/TGUI/pull/112))
        Added setTextSize function to Widget and Gui to allow changing text sizes globally
        Improved TextureManager to only load image once if different parts of image are requested
        Index as optional parameter in SignalItem (which was added in 0.8.6) didn't actually work yet
        MenuItemClicked signal is now also emitted when clicking on menu that has no menu items
        Added option to not replace existing widgets when loading widgets from file
        Added isAnimationPlaying function to Widget (for the show and hide animations)
        Fixed linking issues when compiling TGUI as a static library while dynamically linking SFML
        MousePressed signal in ListBox is now send after the selected item changed instead of before
    - version: 6
      date: 13 October 2019
      changes: |
        Added sort function to ListView to sort data based on values in a chosen column ([PR #107](https://github.com/texus/TGUI/pull/107))
        Added function to Slider to disallow changing the value by scrolling the mouse wheel ([PR #104](https://github.com/texus/TGUI/pull/104))
        Added MultiSelect option to ListView ([PR #113](https://github.com/texus/TGUI/pull/113))
        Added support for text outline in Label and Button widgets
        Added SelectionChanged signal to TextBox
        Added getSelectionStart and getSelectionEnd functions to TextBox
        Added mousePressed and mouseReleased to respond to different mouse buttons in custom widgets
        Added focusable property to widgets
        Added TextureBackground property to Label, Panel and ChildWindow renderers
        Added VerticalScrollAmount and HorizontalScrollAmount to ScrollablePanel
        Added functions to set and get scrollbar values in widgets that have a scrollbar
        Added right mouse clicked signals to ClickableWidget (base class for several widgets) and Panel
        Added HeaderClicked signal to ListView
        Addded Signals namespace with strings for signals of all widgets
        Added VerticalScroll property to Slider, Scrollbar and SpinButton, for more intuitive usage
        Added SubwidgetContainer class that should simplify combining widgets for a custom widget
        Added view to Canvas
        PDB files are now included for Visual Studio builds
        Renamed TimeToDisplay to InitialDelay in ToolTip
        SignalItem (used by ListBox and ComboBox) can now have the item index as optional parameter
        Container widgets didn't pass right click event to child widgets
        Widget state was incorrect when starting a show/hide animation while another was still busy
        Disabling tabs widget caused selected tab to be deselected
        Vertical alignment in Label didn't work correctly when there was a scrollbar
        Knob never responded to mouse events on places where the background texture was transparent
        Text color wasn't updated in MenuBar when disabling and re-enabling widget
        Unicode text wasn't properly handled when loading/saving widgets from/to a widget file
        Fixed potential crash when creating a ProgressBar
        ListView clipped content in expanded column
    - version: 5
      date: 6 April 2019
      changes: |
        Big improvements to Gui Builder
        Svg images are now supported
        ComboBox can now contain some text when no item is selected
        Added function to ComboBox to disallow changing the selected item by scrolling the mouse wheel
        Added RightClicked signal to ListView
        Added functions to ListView to change existing items
        Support typing tabs in TextBox (if tab usage is disabled in gui)
        Added function to signals to temporarily disable callbacks
        Added addition and subtraction operators to Outline
        ChildWindow can now have a different border color in focused state
        Added function to select item in TreeView
        EditBox::setInputValidator now returns false when regex was invalid
        Let ComboBox send the ItemSelected event only after the mouse is released
        TitleBarHeight property of default renderer was ignored in ChildWindow
        Label didn't ignore events after ignoreMouseEvents was called
        Adding space around widgets in Grid to fill the given size wasn't working properly
        Loading widget from file failed when min or max was used in layout strings
    - version: 4
      date: 23 February 2019
      changes: |
        Added Changed signal to CheckBox and RadioButton (to more easily combine Checked and Unchecked)
        Added EscapeKeyPressed signal to ChildWindow
        ExpandDirection of ComboBox can now be set to Automatic
        Added min and max functions to layouts again
        Added horizontal grid lines to ListView
        Added option to ListView to expand the last column to fill the remaining space
        Allow a separator between the header and contents in a ListView
        Split separator in ListView into separator and vertical grid line
        Fixed corrupted white theme when DefaultTheme was initialized before Color constants
    - version: 3
      date: 27 January 2019
      changes: |
        ListView widget added
        EditBox can now have a suffix
        TextBox can now have a horizontal scrollbar
        Label can now have a vertical scrollbar
        Default scrollbar width wasn't always taken from texture size in widgets containing scrollbars
        Scrollbar wasn't drawn correctly when Maximum equaled ViewportSize with AutoHide disabled
        Default icons in TreeView didn't change color when item was selected
        Rounded icon position in TreeView to avoid bad icon alignment
        TreeView didn't handle opacity and font changes
        Sprites didn't keep their transparency when resized
        Texture filenames can now contain UTF8 characters on linux
        Added propery to widget renderer to set an opacity for the disabled state
        Fixed some bugs in saving and loading widget files ([#90](https://github.com/texus/TGUI/issues/90))
    - version: 2
      date: 16 December 2018
      changes: |
        TreeView widget added
        Text styles of lines in ChatBox can now be changed
        Clipping was broken when using multiple windows
        ScrollbablePanel didn't fully scroll to right with both scrollbars visible
    - version: 1
      date: 15 October 2018
      changes: |
        Submenus are now supported in MenuBar
        Menus can now be disabled in MenuBar and given a different text color
        You can now connect a signal handler to a single menu item in MenuBar
        ChildWindow position can be locked to disable dragging it
        Scrollbar thumb should not become smaller than the scrollbar width
        Percentage in layout no longer includes the outline of the parent
        MenuBar didn't work when moved and inverted menu direction was broken
        Text size in MenuBar was reset when changing font
        Handle delete button on android correctly when using SFML >= 2.5
        ChildWindow callback with unbound parameter caused crash
    - version: 0
      date: 5 August 2018
      changes: |
        Global default text size for more consistent texts in widgets
        Gui Builder was added
        A theme can be made the default to use it for all new widgets
        Renderers are decoupled from widgets, making them truly shared
        BitmapButton widget to have an icon next to the button caption
        RangeSlider widget to have two thumbs on a slider
        ScrollablePanel widget to have a Panel with automatic scrollbars
        Panel widget was split in Group, RadioButtonGroup and Panel widgets
        HorizontalLayout, VerticalLayout and HorizontalWrap to arrange widgets
        Relative layouts were improved
        Many other improvements
  - version: 7
    patches:
    - version: 8
      date: 5 August 2018
      changes: |
        EditBox::setInputValidator now throws instead of crashing on GCC < 4.9
    - version: 7
      date: 21 April 2018
      changes: |
        Fixed EditBox InputValidator in textEntered when text was selected
        Hover image of EditBox wasn't being used in Black theme
        Loading widgets from theme was partially broken on android
        ChatBox::setScrollbar didn't correctly position the scrollbar
        ChildWindow::setTitleButtons didn't set the button positions
        ComboBox::setListBox removed all items from combo box
        Added support for using SFMLConfig.cmake to find SFML
    - version: 6
      date: 25 November 2017
      changes: |
        Fixed _fullpath function not found in some MinGW versions
        Knob did not display correctly when using textures
        Fixed crash in layouts in specific case with GCC 7
        ChildWindow::setTitleButtons did not accept combinations of buttons
        Theme files missed MinimizeButton and MaximizeButton in ChildWindow section
        ComboBox::setItemsToDisplay did not show full list when 0 was passed
        Non-string layouts are now also copied when copying a widget
        OpenGL is no longer used for clipping
        Fixed crash that could occur when performing a division by 0 in the layouts
    - version: 5
      date: 11 September 2017
      changes: |
        Added closeMenu function to MenuBar
        System clipboard is now used on all platforms when using SFML >= 2.5
        Fixed FindTGUI.cmake script when patch version is not specified
        Label didn't send a SizeChanged signal when its text changed
        The size of a Grid was reset in removeAllWidgets
        Holding shift and pressing arrow keys will select text in EditBox
        Fixed syntax error in BabyBlue and TransparentGrey themes
        Resource path was not correctly used for all resources
        Picture::create did not store the filename used
    - version: 4
      date: 3 April 2017
      changes: |
        Theme and widget file parser now provides line number on error
        Scrollbar in ListBox remained in hover state until mouse left the ListBox
        The position of TextBox could be wrong when using layouts
        Fixed incorrect clipping when the viewport was changed
        Optional parameter from Checked/Unchecked signal could have been wrong
    - version: 3
      date: 9 February 2017
      changes: |
        Added create function to widgets and Theme which is now the preferred way to construct them
        Added setDefaultTextStyle to EditBoxRenderer
        Touch events were not handled properly when not using the default view
        Optimized EditBox::findCaretPosition
    - version: 2
      date: 2 December 2016
      changes: |
        EditBox did not send a TextChanged signal when pressing ctrl+X
        Added optional maximize and minimize buttons to ChildWindow ([#61](https://github.com/texus/TGUI/pull/61))
        Calling Picture::setTexture yourself now works properly
        Added optional parameter to Widget::disable to let mouse events pass through to the widgets behind it
        ChatBox didn't scroll down automatically when size was not a multiple of item heigh
        Clipping in ChatBox did not take padding into account
        Mouse wheel scroll on top of combo box changed the item internally but did not display the new item
        When ComboBox was destroyed while the list was still open then the list remained visible
        Fixed invalid memory reads when widget gets destroyed from inside a callback function
        Allow the ChildWindow to have maximum and minimum sizes ([#64](https://github.com/texus/TGUI/pull/64))
        Removed support for 32-bit on OS X
        Fixed texture rotation in some rare cases
        Menus in MenuBar didn't stay open when the menu bar was added inside a Panel
        Added EditBox::getCaretPosition
        Fixed clipping issues when not using the default view
        getAbsolutePosition no longer takes the position of the gui view into account
    - version: 1
      date: 12 June 2016
      changes: |
        Picture can now be loaded from a part of an sf::Texture
        Any texture in any widget can now be set to an sf::Texture
        Optimized adding lines to ChatBox
        You can now pass <i>const char*</i> directly to layout (instead of needing std::string)
        Panel widgets can now have borders around them
        Setting texture color did not work when using transparent widgets
        Passing std::bind expression as parameter to the connect function did not work on VS2013
        Fixed tab key not working for widgets inside several containers
        Fixed crash in Signal class under specific circumstances
    - version: 0
      date: 4 April 2016
      changes: |
        Experimental Android and iOS support
        Layout system for relative sizes and positions
        Rewritten callback system
        New loading system to share theme between widgets and allow customizing loading steps
        Exceptions are now used for error handing
        No longer only textured widgets, also colored widgets which do not need external files
        Many, many more changes
  - version: 6
    patches:
    - version: 10
      date: 24 January 2016
      changes: |
        Improved vertical text alignment in ListBox
        Fixed RadioButton images being used when CheckBox was loaded from widget file
    - version: 9
      date: 11 July 2015
      changes: |
        Allow loading custom sections in theme files
        Smooth scrolling in Slider and Scrollbar
        Added setAlignment function to EditBox
        Fixed clipping when drawing on Canvas right before drawing the gui
        Widget was not given a parent when it was copied
        SpinButton did not send mouse click callbacks
        Link to Headers inside mac framework had an absolute path
        Fixed scaling of form builder window
        Limit the amount of visible items in the widget selector in form builder
    - version: 8
      date: 10 May 2015
      changes: |
        Fixed segfault during handleEvent in a rare situation
        Removed warnings in ListBox when index was too high
        References from getWidgets and getWidgetNames should be const
        Worked around a syntax error when using SWIG ([#37](https://github.com/texus/TGUI/issues/37))
    - version: 7
      date: 7 February 2015
      changes: |
        Added support for frameworks on mac os x
        Added setTextStyle and getTextStyle to Label
        Allow widget changes before they are added to the gui
        Completely cache theme files on first access
        Fixed loading/saving in form builder on mac os x
        Fixed potential crash in VS2012
    - version: 6
      date: 21 September 2014
      changes: |
        Cache theme files for faster loading
        Added method to load a Picture from an sf::Texture
        You can no longer try to copy the Gui object
        First selected newline in TextBox wasn't being copied with ctlr+c
        Fixed variable in ListBox not being copied
        Dragging events were not passed through container widgets
        List from ComboBox did not shrink when removing items
        Fixed crash when removing widget in a specific situation
    - version: 5
      date: 13 August 2014
      changes: |
        Added FindTGUI.cmake
        Added recursive search with widget name
        Text can now be selected with ctrl+A
        Lines in ChatBox can now start from bottom
        Worked around bug in SFML 2.1 in Label
        Fixed bug in ChatBox with displaying some lines
        Fixed panel keeping hover state of widget
        Fixed potential crash when removing a widget
        Fixed bug with saving and loading in form builder
    - version: 4
      date: 28 June 2014
      changes: |
        Improved auto-sizing of text
        Allow rendering to a RenderTarget
        On windows the system clipboard will be used
        ListBox and ComboBox now have ids for their items
        ListBox and ComboBox now have methods to change items
        Fixed crash in TextBox when no font was set
        Fixed crash in EditBox when LimitTextWidth was set
        ScrollAmount now also taken into account when using the mouse wheel
        Title text in ChildWindow wasn't vertically centered
        Theme files edited on windows couldn't be loaded on unix
        ListBox::setSelectedItem should move the scrollbar if needed
        ChatBox didn't display correctly when text got split over multiple lines
    - version: 3
      date: 11 April 2014
      changes: |
        Added canvas widget to render sfml inbetween tgui widgets
        Add a function to TextBox to make it read-only
        Clipboard calculations didn't take viewport into account
        Fixed Tab text being wrongly clipped when tab width is too small
        BabyBlue theme didn't use correct EditBox images
        SharedWidgetPtr class was lacking a working == and != operator
    - version: 2
      date: 14 March 2014
      changes: |
        Fixed potential bug in setMaximum and setMinimum in Slider and Knob
        Fixed Knob widget not being displayed correctly when scaled
        Fixed invisible title bar of child window when setSize was not called
    - version: 1
      date: 9 February 2014
      changes: |
        Made several fixes to Scrollbar
        Checkbox now inherits from RadioButton instead of the other way around
        Text was also being selected in TextBox when clicking on the scrollbar
        Some fonts didn't display all lines in ChatBox
        Allow setting a default text color in ChatBox and passing different parameters to addLine
        Allow setting the line spacing in ChatBox
        Added a resetView parameter to handleEvent and set the default value on true (for draw function as well)
        Implemented getFullSize and thus changed behavior of getSize
        The TabKeyUsageEnabled property should now be accessed through a function call
        Implemented setResourcePath function to load everything from a relative path
        Support clang on linux
    - version: 0
      date: 16 January 2014
      changes: |
        Many things changed, no list of changes was maintained.
  - version: 5
    patches:
    - version: 2
      date: 17 May 2013
      changes: |
        Added ChatBox class.
        Fixed a bug in LoadingBar when minimum wasn't 0.
        Fixed a bug in slider when calling setVerticalScroll after setSize.
    - version: 1
      date: 8 February 2013
      changes: |
        Added support for changing the title alignment in ChildWindow.
        Fixed a minor bug with unfocusing objects.
        Fixed a small bug in the CMake script.
    - version: 0
      date: 23 December 2012
      changes: |
        New objects: ChildWindow, AnimatedPicture, SpinButton, Tab, Grid and more.
        Changed the add, get and copy functions from Group. This allows creating custom objects.
        Serious speed increase, especially with many objects or with ListBox, ComboBox and Label.
        ComboBox::load now also takes the height as parameter.
        EditBox and TextBox take the selection point color as extra parameter in changeColors.
        Improved documentation and tutorials.
        Panel can optionally load a background image.
        Objects in panel can now be focused with the tab key.
        You can now choose to only draw part of the label.
        Label has an optional background color.
        Double clicking in EditBox and TextBox now selects the text.
        Panel and ChildWindow can also load their objects from the Object Files.
        You can choose to disable the tab key usage.
        An easy install script was added for linux users.
        A LoadingBar can now have text on top of it.
        Callback now contains a pointer to the object.
        Unicode support through the use of sf::String.
        Many bug fixes.
  - version: 4
    patches:
    - version: 2
      date: 16 July 2012
      changes: |
        Bug fix: ComboBox wasn't responding correctly when scaled.
        Bug fix: ComboBox wasn't rendered correctly when using a scrollbar.
    - version: 1
      date: 15 July 2012
      changes: |
        Bug fix: Slider caused a runtime error on windows.
        Bug fix: Objects didn't behave correctly when the view was changed.
        Bug fix: When all the objects were hidding, the first one was still drawn.
    - version: 0
      date: 13 July 2012
      changes: |
        New objects: Panel, TextBox and SpriteSheet.
        Objects can be moved to the back or to the front.
        Scrolling in listbox has been improved (you can now set the scrollbar between two items).
        The images used in Checkbox can now have a different size.
        Hover images no longer have to be semi-transparent.
        Text width can now be limited in EditBox.
        A maximum item limit was added in ComboBox.
        You can now disable objects.
        Many bug fixes.
  - version: 3
    patches:
    - version: 7
      date: 5 April 2012
      changes: |
        Mac OS X is now supported (no libraries yet, but the source code can be used)
        Changing the global font no longer changes the font of already existing objects.
        You can now change the global font directly, the setGlobalFont function was removed.
        All objects now have a getSize and getScaledSize function.
        Label now sends a callback on click.
        You have an option too keep the scrollbar visible, even if you can't scroll.
        Bug fix: The getText function of EditBox returned a sf::String instead of an std::string.
        Bug fix: Arrow keys were not repeated in EditBox.
        Bug fix: Texture rect was not updated when calling the load function again.
        Bug fix: Callback from Picture didn't work with scaling.
        Bug fix: EditBox and ListBox hung when image was too small.
        Bug fix: Alpha value in colors was sometimes wrong.
        Bug fix: The displayed text in EditBox was wrong when calling setMaximumCharacters.
        Bug fix: ComboBox caused a crash when image was too small.
    - version: 6
      date: 13 March 2012
      changes: |
        A new object: Label.
        Bug fix: The draw function was overriding the one from sfml, it is now called drawGUI.
    - version: 5
      date: 12 March 2012
      changes: |
        The 'm_' prefix was dropped on public members.
        Picture now has a click callback.
        The background color of the selected text is now different when the EditBox is not focused.
        EditBox now also sends a callback when the return key was pressed.
        A new object: Panel.
        Bug fix: The text on the button was sometimes blurry with fixed text size.
        Bug fix: The arrow of ComboBox kept pointing down when the list was shown.
    - version: 4
      date: 10 March 2012
      changes: |
        setSize function added to all gui objects. It is recommended over the SetScale function.
        You can now change the font of EditBox, ListBox and ComboBox.
        A setGlobalFont function was added to Panel (also to Window) to set the font of all the objects.
        You can now add CallbackID to any object that is loaded from a file.
        A horizontal scrollbar has been added.
        Bug fix: Objects behind a picture still received events.
        Bug fix: Thumb position of Scrollbar was wrong while scaling.
        Bug fix: The program crashed after calling removeAllObjects.
    - version: 3
      date: 6 March 2012
      changes: |
        Scrolling was added in EditBox, there is no more text width limit by default.
        You can now get the colors that are used in EditBox, Listbox and ComboBox.
        You can now select an item in ListBox with the new setSelectedItem function.
        You can now use the scrollbar.
        Panels can now be used, which means that you can create multiple groups of linked radio buttons.
        You can now load all the objects (except Panel) from a file.
        Except for the .lib and .so files, I have included .a files.
        Bug fix: You couldn't call setBorders after setText in EditBox.
        Bug fix: Changing the maximum items and the item height in ListBox didn't work properly.
        Bug fix: Scrollbar didn't receive the mouse up event when releasing the mouse on ListBox.
        Bug fix: Calling a load function with an empty string made the program crash.
    - version: 2
      date: 20 February 2012
      changes: |
        ComboBox was added
        Bug fixes in EditBox, ListBox and Scrollbar
    - version: 1
      date: 15 February 2012
      changes: |
        Namespace was changed to lowercase.
        A setVerticalScroll function for changing m_VerticalScroll indirectly.
        Changed the name from setTextColors to changeColors inside EditBox to conform with the ListBox.
        Small bug fixes in EditBox and Listbox.
        Fixed wrong behavior of scrollbar.
        Colors of objects were changed to fit better with each other.
    - version: 0
      date: 12 February 2012
      changes: |
        First public release.
---

{% for major in page.changelog %}
  {% for minor in major.minors %}
    {% for patch in minor.patches %}
      {% capture version %}{{major.version}}.{{minor.version}}.{{patch.version}}{% endcapture %}
<h3 id="{{version}}">TGUI {{version}} <span class="ChangelogDate">({{patch.date}})</span></h3>
        {% assign changes = patch.changes | newline_to_br | strip_newlines | split: '<br />' %}
        {% for change in changes %}
- {{change}}
        {% endfor %}
    {% endfor %}

    {% if forloop.last == false and major.version < 1 %}
<br>
<hr>
    {% endif %}
  {% endfor %}

  {% if forloop.last == false %}
<br>
<hr>
  {% endif %}
{% endfor %}
