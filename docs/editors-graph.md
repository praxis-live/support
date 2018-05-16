# Graph editor

![Graph editor](img/graph.png)

The default editor is the graph editor, used for audio, video, tinkerforge, and
generic data pipelines. Components and port connections are represented in a graphical
node form that should be familiar to anyone who has used other patcher-style environments.

To **add components**, drag the component type you want from the `Palette` window
onto the editor, or use the popup menu browser. Components are automatically filtered
depending on the root type. You will be asked to name the component (a unique name
will be suggested) – _names cannot be changed once the component is created_.

To **connect ports**, click and drag with the mouse between the two ports. You cannot
connect ports of incompatible types, and output ports can only be connected to input ports.
Connections are colour-coded depending on type.

Components and port connections can be removed by selecting them and pressing `Delete`
or selecting delete from the pop-up menu.

Selected components may be moved around by dragging with the mouse, or using the cursor keys.
Selected components may also be copy & pasted. Multiple components can be selected by clicking
and dragging a selection box around them, or by `CTRL-clicking` on each component.

To **control or edit component properties**, double-click on the component to open
the component editor window. Component actions are also accessible through the pop-up
menu on components. When selected, component properties also show up in the `Properties`
tab – if multiple components are selected you can use this tab to control all of them together.

**Zoom in and out** of the graph using `CTRL-mousewheel`, and **navigate** around using 
the scroll wheel or satellite view in the bottom-right of the editor.
