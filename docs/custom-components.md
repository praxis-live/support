# Custom components

_PraxisLIVE_ allows components to be defined and re-defined at runtime. However,
_you do not need to code to benefit from this_. There is a growing
repository of custom components that you can use in your projects.

Custom components are defined in PXG (`.pxg`) files. A PXG file can also contain multiple
components and their connections, so you can also create your own files for sharing common 
configurations of components the same way.

The Dashboard can install components from the following repository automatically
into your palette, or you can download them manually.

[https://github.com/praxis-live/components](https://github.com/praxis-live/components)

## Import a PXG file

To import a custom component into your project that isn't installed in the palette,
simply drag the file from the `File Browser` tab into your graph editor.

If you find yourself using a custom component regularly, you can also add the `.pxg`
file to the `Palette`. Use the pop-up menu on a palette category and select `Import ...`.
Make sure to use a suitable category, remembering that some categories will be filtered
depending on the type of graph you are editing.

Unlike built-in components, any code that defines custom components will be stored
in your project. This keeps projects self-contained - you do not have to copy the
PXG files alongside your project.

## Export a PXG file

To create a `.pxg` file, simply select the component(s) you want to include and
select `Export` from the pop-up menu. You will also be given the opportunity to add
the exported file directly to your palette.
