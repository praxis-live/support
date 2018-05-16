# Custom components

_PraxisLIVE_ allows components to be defined and re-defined at runtime. However,
_you do not need to code to benefit from this new functionality_. There is a growing
repository of custom components that you can use in your projects.

The Start Page will automatically install components from the following repository
into your palette, or you can download them manually.

[https://github.com/praxis-live/pxg](https://github.com/praxis-live/pxg)

## Import a .pxg file

Custom components are defined in `.pxg` files. To import a custom component into
your project that isn't in the palette, simply drag the file from the `File Browser`
tab into your graph editor. _Custom components are only currently supported in the
graph editor_.

If you find yourself using a custom component regularly, you can also add the `.pxg`
file to the `Palette`. Use the pop-up menu on a palette category and select `Import ...`.
Make sure to use a suitable category, remembering that some categories will be filtered
depending on the type of graph you are editing.

Unlike built-in components, the code that defines custom components will be stored
in your project. This keeps projects self-contained - you do not have to copy the
custom component files alongside your project.

## Export a .pxg file

To create a `.pxg` file, simply select the component(s) you want to include and
select `Export` from the popup menu.
