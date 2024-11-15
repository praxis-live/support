# Editors

When a project is built or run, each `.pxr` file will open in an editor. You can also open
files manually. The type of editor a `.pxr` files opens in will depend on the root type 
(video, audio, gui, etc.).

At the top-right of the editor region are four icons. Three of these allow you to
switch between open editors. The maximize icon ![](img/view-fullscreen.png) allows
you to enlarge the editor area, minimizing all the other windows to the sides and
providing more workspace.

All editors have their own toolbar, which contains at least two buttons. The play button
![](img/play.png) toggles whether the root is active. The root properties
button ![](img/properties.png) opens a window allowing you to set properties of the
root component (such as video resolution and frame-rate). _Not all root properties
can be set while the root is active_.

You can save a `.pxr` file at any time when the editor is active. You will also be
prompted to save whenever a project is cleaned. Due to its distributed architecture,
_PraxisLIVE_ does not attempt to track all changes so will always prompt you to save.

## Editor types

* [Graph editor](editors-graph.md) (default)
* [GUI control panel editor](editors-gui.md)