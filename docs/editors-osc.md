# OSC bindings editor

OSC `.pxr` files will open in the OSC bindings editor. This is a simple editor
that allows you to connect OSC messages to any _PraxisCORE_ control.

To **add a binding**, use the `Add` button in the editor toolbar.

Bindings are arranged in a table. You can **edit properties** directly within the table.
Choose a control to send incoming OSC messages to. The default OSC address is the same
as the control address, but can be overridden by any valid OSC address value. The same
OSC address can be used in multiple bindings to send values to multiple controls.

To **delete a binding**, select it and press `Delete` or use the delete button in
the editor toolbar.

The **port for receiving OSC messages** can be selected using the root properties. A last
message property can also be used for monitoring incoming messages.
