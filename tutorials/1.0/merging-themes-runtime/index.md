---
layout: page
title: Merging themes at runtime
breadcrumb: merging themes
---

If you are using multiple themes at the same time then you can call `setRenderer` on the widget with a renderer from one of those themes. If for some reason you only want to use a single theme however (e.g. because you want all renderers to be available in the default theme), then you will have to merge the themes somehow. Since themes are linked to files, it isn't straightforward to combine multiple files into a single Theme object.

It can however easily be accomplished by copying some extra loading code that is provided here.

The following piece of code will allow loading the renderers from a theme file, without actually creating a Theme object.
```c++
// The DefaultThemeLoader class handles parsing TGUI theme files, so we inherit from that class for its functionality
class CustomThemeLoader : public tgui::DefaultThemeLoader
{
public:

    // We add a custom function for us to call later
    std::map<tgui::String, std::shared_ptr<tgui::RendererData>> loadRenderersFromFile(const tgui::String& filename)
    {
        // The preload function from DefaultThemeLoader will parse the file and store
        // all data in the m_propertiesCache member.
        preload(filename);

        // We loop over all sections in the theme file and create renderers from them
        std::map<tgui::String, std::shared_ptr<tgui::RendererData>> renderers;
        for (const auto& section : m_propertiesCache[filename])
        {
            const tgui::String& sectionName = section.first;
            const auto& properties = section.second;

            auto renderer = tgui::RendererData::create();
            for (const auto& property : properties)
                renderer->propertyValuePairs[property.first] = tgui::ObjectConverter(property.second);

            renderers[sectionName] = renderer;
        }

        return renderers;
    }
};
```

With the above code, you can create a theme object and insert renderers from multiple files into it:
```c++
auto combinedTheme = tgui::Theme::create();

CustomThemeLoader customThemeLoader;
for (const auto& filename : themeFilenamesToLoad)
{
    auto renderers = customThemeLoader.loadRenderersFromFile(filename);
    for (const auto& pair : renderers)
        combinedTheme->addRenderer(pair.first, pair.second);
}
```
