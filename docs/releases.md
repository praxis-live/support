# Praxis LIVE releases

Details of changes made in each Praxis LIVE release are here and on the [Github release page](https://github.com/praxis-live/praxis-live/releases). Potentially breaking changes are highlighted.

## Praxis LIVE 3.0.0 (27 October 2016)

### Runtime

- **Core**
    - Updated infrastructure and internal compiler to support Java 8. **Java 8 is now a minimum requirement**.
    - Major refactoring and performance improvements to compiler support, in particular support for running the compiler in a separate process when running distributed hubs where the slave requires good low-latency performance or is running on a low-performance device (eg. Raspberry Pi).
    - PBytes data type added to support arbitrary binary data as properties or messages (required by new compiler support). Text representation as Base64.
    - PBytes-backed support added for any Serializable type or List of Serializable types as properties or injected fields in custom code.
    - Added a lightweight HTTP file server (based on NanoHTTP) for serving project files to remote slaves in distributed hubs (off by default).
    - Logging from slaves to master added for better error reporting when working with distributed hubs.
    - New `root:data` root type provided for generic graphs of components / asynchronous tasks. Specific non-real-time data processing components will follow.
    - Fixed default value in `core:routing:every` throwing an exception.
    - Removed most deprecated components. **Projects relying on components deprecated in v2 may need updating.**
    - Deprecated `core:container:property` as start of revision of containers.
    - Moved `core:container:input|output` to `core:container:in|out` so that default names match default port names.
    - Updated `PVector` to version from Processing 3.
    - Added `skew` annotation to numeric properties to allow for better UI representation of values with non-linear response (eg. sliders for audio gain, frequency).
    - Added `link()` method to properties to bind method references or lambdas to changing property values (see usage in new audio components).
    - Added `randomOf()` utility methods to return random value from arrays.
    - Other Java 8 functional improvements made to core types (eg. `PArray.stream()` )
    - Removed `-J-Xincgc` option from configuration to switch from the deprecated incremental garbage collector back to the default (`-J-Xincgc` or other GC options may be specified as a launch argument if required).
    - Updated JNA library to 4.2.2 and added JNA Platform library.
- **Video**
    - Switched video capture and playback support to GStreamer 1.x and removed support for GStreamer 0.10. **GStreamer libraries for Windows and OSX are no longer included by default and must be installed separately as described in the documentation. Custom pipelines may need revising.**
    - OpenGL renderer performance improvements, including full GL acceleration of _all_ blend modes in `video:composite` and `video:xfader`.
    - Fixed `video:gl:p3d` adding an alpha channel to opaque surfaces. **Projects that relied on this incorrect behaviour may need some changes.** 
- **Audio**
    - Revised audio components now stereo and re-codeable by default, with link ports removed. Older audio components have been deprecated and will be removed. **`audio:gain` no longer has a link port so projects may need reconfiguring.**
    - Improved API for custom coding audio DSP based on Java 8 functions.
    - Changed `Delay` UGen in custom coding to passthrough by default. **Custom audio components using delays may need revising.**
    - Changed `IIRFilter` UGen to not expose type enum from underlying JAudioLibs library. **Custom audio components using filters may need revising.**
- **Custom GUIs**
    - New simple look and feel for custom UI components, including improvements for live interaction (eg. sliders can be dragged from anywhere, not just on thumb). This custom UI is designed to support user-defined colouring in a later release.
    - Moved custom look and feel setup into `root:gui` to reduce resource usage with slaves / command line player when the UI is not required.
    - Added support for new `skew` annotation on sliders and XY-pads.
    - Switched `gui:range-slider` to use two single bindings for low and high, as single property ranges are not generally supported. **Existing projects using range sliders will need bindings to be updated.**
- **OSC / MIDI bindings**
    - provide `last-message` property to see incoming messages.

### IDE

- **Core**
    - Updated base IDE platform to NetBeans 8.1.
    - Switched to Darcula look & feel - better UI consistency and OSX menu support.
    - Improved display of current version on Start Page and About dialog.
    - Moved user configuration directory to `.praxislive3` - **Any custom configuration will need recreating**.
    - Moved default projects directory to `PraxisLIVE Projects`.
    - Improved dialog for importing single custom components (commonest usage) by using a simple text field rather than a table.
    - Changed default component naming to always add a hyphen before the number to match usual port numbering.
    - Added syncing when copying to clipboard to ensure property values are up-to-date.
    - Changed palette to only display simple name and fixed names when moving components into different palette categories.
    - Added settings in `Options|General|Network` for configuring compiler, http server, etc. when using distributed hubs.
    - Added settings under `Options|Video|GStreamer` for configuring location of the GStreamer install (only required for custom installation locations).
- **Graph editor**
    - Added ability to create components from scene popup menu (`Add`) as quicker alternative to dragging from palette.
    - Added component export action (in popup menu).
    - Added snap alignment.
    - Added mouse wheel scrolling support.
    - Added focus highlighting and keyboard focus control.
  
## Praxis LIVE v2.3.3 (7 July 2016)

- FIX crash on some (NVidia) graphics cards when recoding `video:gl:p3d` components.
  
## Praxis LIVE v2.3.2 (13 May 2016)

- fix issue with new `video:gl:p2d` component leaking matrix transforms affecting downstream components.
- fix issue with new `video:gl:p2d` component not remembering style settings from setup() and between calls to draw()
- fix issue with straight lines not rendering in `video:custom` component when using OpenGL rendering.
- add texture matrix and offset uniforms when setting a PImage uniform in custom shaders (technically a new feature!)
- fix commenting not working in GLSL editor.

## Praxis LIVE v2.3.1 (17 March 2016)

- Fix major issue with editing projects with containers in them (praxis-live/support#26)

## Praxis LIVE v2.3.0 (17 March 2016)

- New `video:gl:p2d` component wrapping Processing's 2D OpenGL graphics. Provides better performance (doesn't require an image copy) and more accurate rendering where 3D capability is not required.
- Updated shader support allows custom shaders to be used when rendering shapes as well as images.
- New ability to add comments and custom colouring in the graph editor (praxis-live/support#12 and praxis-live/support#16)
- Improved reporting of exceptions thrown in user code (praxis-live/support#13)
- Revised start page and update checking mechanism. Also, all external links to docs and issues are managed through praxislive.org domain (praxis-live/support#21)
- `video:player` now supports playback rate and audio output, but currently only when using GStreamer 1.x (non-default option in `Tools | Options` which requires installation of GStreamer 1.x library). Using JACK it is possible to route audio from a video through an audio processing pipeline.
- FIX : custom shaders are now pre-processed correctly like internal Processing shaders to match the required GLSL version. Should fix issue with errors on OSX (praxis-live/support#15)
- FIX : the main project file is now updated whenever a change is made, rather than at application shutdown (praxis-live/support#19)
- FIX : containers now register correctly as deleted (praxis-live/support#18)
- FIX : ShapeRender (finally) fixed for some blend modes when software rendering (praxis-live/support#1)
- FIX : gst1-java-core library updated to later build with fix for loading GStreamer 1.x on OSX

## Praxis LIVE v2.2.0 (28 October 2015)

- OpenGL video pipeline updated to Processing v3.0.1 / JOGL v2.3.2. While providing major performance and stability improvements, this is a major change and there is the possibility for some regressions. Please update carefully and report any issues.
- Optional support for GStreamer 1.x. This can be set under `Tools / Options / Video / GStreamer` (requires restart). Unlike GStreamer 0.10 support, this requires a system installed version of the GStreamer library on all platforms. Windows and OSX users who want to experiment with this feature can download GStreamer from http://gstreamer.freedesktop.org/download/
- New `core:tracker` component, and simple table-based tracker editor (use popup menu and `Edit patterns` to access). More advanced editing features to follow.
- New routing components - `core:routing:every` for allowing through every n-th message, and `core:routing:order` to prioritise dispatching of messages.
- New `audio:clock` component for more stable timing in BPM (quantized to internal processing buffer size).
- Internal (and outdated) help removed and replaced with link through to online manual at http://praxis-live.readthedocs.org
- TinkerForge bindings updated to v2.1.5.
- Added `ready` and `error` ports to `video:capture` and `video:player`. Can be connected to `play` or `pause` to auto-start playback when new file or pipeline loaded.
- Many minor bug fixes (see commit log).

