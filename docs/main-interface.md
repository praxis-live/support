# Main interface

The image below outlines the default window layout. _PraxisLIVE_ uses a typical MDI interface, whereby components and documents being edited are encompassed within a single main window. It is possible to drag components around the interface or un-dock them into a separate window. Any changes you make will be persisted – if you want to reset to the default layout, use the `Window / Reset Windows` action in the main menu.

![Main window layout](img/window-layout.png)

## Toolbar

Six common actions have buttons in the main toolbar.

* **New Project** ![](img/newProject24.png)

   Start the new project wizard.

* **Open Project** ![](img/openProject24.png)

   Open the project browser allowing you to open a pre-existing project. The project will open in the `Project` tab, but will not be run.

* **Save File** ![](img/save24.png)

   Save the file currently being edited. **NB. it does not save the entire project.**

* **Clean (stop) Project** ![](img/cleanProject24.png)

   Clean the selected project. All roots will be stopped and processes shutdown. A dialog will be opened
giving the option to save any associated files first.

* **Build Project** ![](img/build.png)

   Build the selected project. This will install any root components in the hub (so they can be edited), but will not run them (play audio, video, etc.)

* **Run Project** ![](img/run.png)

   Run the selected project. This will build the project (if necessary) and then start all roots set to run automatically.

## Projects / File browser

The `Projects` tab provides an interface for opening and managing projects. Projects are shown in a tree-like
structure giving access to all the user-editable files that make up a _PraxisLIVE_ project.

The `File browser` tab provides a mechanism for browsing the local file system.

## Hub manager

The `Hub Manager` tab provides an overview of the roots running within the _PraxisLIVE_ hub.

The LED icon to the left of the root ID shows whether a root is running or not. Individual roots can be started 
or stopped by double-clicking.

## Properties / Palette

The `Properties` tab provides a way to view and edit the properties of the currently selected item(s), such as a
file in the `Projects` tab or a component in the the editor.

The `Palette` shows the range of components that may be added to the root file currently being edited. The Palette
tab will only show up when a relevant file is open for editing.

## Options / Settings

The `Options` window can be opened through the `Tools` menu in the main menu bar (or the app menu on macOS). It
provides the ability to set common global preferences such as update checking, keyboard shortcuts, GStreamer location, etc.

