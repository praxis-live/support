# Custom components

The new code infrastructure introduced in _Praxis LIVE v2_ allows components to be defined and re-defined at runtime. However, _you do not need to code to benefit from this new functionality_. There is a growing repository of custom components that you can import into your projects.

[https://github.com/praxis-live/pxg](https://github.com/praxis-live/pxg)

## Import a .pxg file

Custom components are defined in `.pxg` files. To import a custom component into your project, simply drag the file from the `File Browser` tab into your graph editor. _Custom components are only currently supported in the graph editor_.

If you find yourself using a custom component regularly, you can also add the `.pxg` file to the `Palette`. Use the pop-up menu on a palette category and select `Import ...`. Make sure to use a suitable category, remembering that some categories will be filtered out depending on the type of graph you are editing.

Unlike built-in components, the code that defines custom components will be stored in your project itself. This keeps projects self-contained - you do not have to copy the custom component files alongside your project.

## Create a .pxg file

You may notice that the text of a `.pxg` file is very similar to that of a `.pxr` file - in fact, a `.pxg` file is effectively a sub-graph. These files are not just for custom components, and _you are not limited to a single component in the file_. They may be used to export whole configurations of components for re-use.

There is not currently an export function in the graph editor. However, it is very easy to create a `.pxg` file. Simply select the component(s) you want to include and copy & paste them into a text editor of your choice (eg. `CTRL-c` in the graph, then `CTRL-v` in your text editor). Make sure to save the file with a `.pxg` extension.
