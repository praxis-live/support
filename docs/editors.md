# Editors

Double-clicking on a `.pxr` file will open it in an editor. The type of editor it
opens in will depend on the root type (video, audio, gui, etc.). Remember that the
project the file belongs to must have been built so that the root shows up in the
`Hub Manager` before the file can be edited, otherwise the editor will contain a
message asking you to build the project.

At the top-right of the editor region are four icons. Three of these allow you to
switch between open editors. The maximize icon ![](img/view-fullscreen.png) allows
you to enlarge the editor area, minimizing all the other windows to the sides and
providing more workspace.

All editors have their own toolbar, which contains at least two buttons. The play button
![](img/play.png) toggles whether the root is playing / active. The root properties
button ![](img/properties.png) opens a window allowing you to set properties of the
root component (such as video resolution and frame-rate). _Not all root properties
can be set while the root is playing_.

You can save a `.pxr` file at any time when the editor is active. You will also be
prompted to save whenever the hub is restarting or when a root is removed from the hub.
Due to its distributed architecture, _PraxisLIVE_ does not attempt to track all changes
so will always prompt you to save.

## Editor types

* [Graph editor](editors-graph.md) - audio, video, TinkerForge and generic data graphs.
* [GUI control panel editor](editors-gui.md)
* [MIDI bindings editor](editors-midi.md)
* [OSC bindings editor](editors-osc.md)