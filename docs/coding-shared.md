# Shared code

PraxisCORE supports shared, rewritable code across components within a single
root (eg. within one graph). Sources for shared code are stored as a map property
on the root component. All shared types are in the `SHARED` package.
[Data ports](coding-data-pipes.md) can pass shared types between components.

Every time you edit and save a shared code type, all dependent components using
any shared code will be recompiled and updated atomically. If a code edit or type
deletion causes a compilation error in the shared code or any component, the old
iteration of code will continue to be used.

Updating the code of a component using shared code will not cause other
components or shared code to be recompiled (components continue to be isolated
in separate classloaders that have the shared code as a parent).


## Using shared code

To access the UI panel for shared code, right-click on the graph background and
toggle `Shared Code`.

To create a new shared code type, right-click on the `SHARED` folder and select
`New Type...`. The name must be a valid Java type name. A class will be created
by default and opened in the editor. It can be changed from class to interface,
enum, etc. if required.

Shared code files are plain Java types, and have none of the default methods or
imports that components have.

To access shared code from a component's code, add an import - eg.

```
import SHARED.Foo;
import static SHARED.Utils.*;
```

You can `CTRL-click` any shared code type or method to open it in the editor.

!!! important
    Just like with normal component code, saving the code updates the components
    in memory - make sure to save the graph or project to disk!
