# Video components

## video:capture

Video capture support for using webcams, etc. You can use the default auto source,
specify a device index (preferred, cross-platform) or a GStreamer pipeline. The pipelines
for each device index can also be configured globally under `Tools / Options / Video / GStreamer`.

It is possible to specify the input dimensions and framerate.

A capture device will not start automatically. You can attach a `core:start-trigger` to the `play` port.

The device property is empty by default. It must be set before capture can be started.

- **out** : _VideoOut_
- **device** : _Property (empty, number 1..4 or string)_ : device index or gstreamer pipeline or empty for the auto source
- **resize-mode** : _Property (Crop, Stretch, Scale)_ : control how the video capture size is adapted to the output size
- **align-x** : _Property (number 0..1)_ : horizontal alignment (only when using resize-mode Crop or Scale)
- **align-y** : _Property (number 0..1)_ : vertical alignment (only when using resize-mode Crop or Scale)
- **source-width** : _Property (empty or number 1..)_ : capture width
- **source-height** : _Property (empty or number 1..)_ : capture height
- **source-fps** : _Property (empty or number 1..)_ : capture framerate
- **play** : _Action_
- **stop** : _Action_

## video:composite

Compose the video input source onto the video input destination using the blend mode and additional opacity as set.

The blend modes available are Normal, Add, Sub, Difference, Multiply, Screen, BitXor

- **in** : _VideoIn_ : destination input
- **src** : _VideoIn_ : source input
- **out** : _VideoOut_
- **mode** : _Property_ : blend mode from above list
- **mix** : _Property (number 0..1)_ : additional opacity from 0% to 100%
- **force-alpha** : _Property (boolean)_ : force the source input to have an alpha channel (transparency)

## video:custom

Base for video components. See coding guide.

- **out** : _VideoOut_

## video:output

Output to screen. An output is required in a video graph.

Currently settings here are only read when the video graph is started. Changing them will not affect a running video graph.

**Width and height set here only scale output. They do not affect the size of image being processed through the graph - use the root settings for that.**

- **title** : _Property (empty or string)_ : title of window or empty for default
- **device** : _Property (empty or number 1..)_ : monitor device or empty for default
- **width** : _Property (empty or number 1..)_ : output width or empty for default (see note above)
- **height** : _Property (empty or number 1..)_ : output height or empty for default (see note above)
- **rotation** : _Property (empty or number 0,90,180,270)_ : rotation of output on screen
- **full-screen** : _Property (boolean)_ : display output full screen
- **always-on-top** : _Property (boolean)_ : keep output window above other windows on screen
- **undecorated** : _Property (boolean)_ : remove output window decorations

## video:player

Video file player.

It is possible to pause and seek through a video file using the `position` property. For this purpose use a video codec such as MJPEG which supports discreet frames.

The player will not start automatically. You can connect a `core:start-trigger` to the `play` or `pause` ports.

- **out** : _VideoOut_
- **video** : _Property (empty or resource)_ : video file location or empty to clear
- **position** : _Property (number 0..1, transient)_ : position within video file (normalized)
- **loop** : _Property (boolean)_ : loop playback
- **resize-mode** : _Property (Crop, Stretch, Scale)_ : control how the video frame size is adapted to the output size
- **align-x** : _Property (number 0..1)_ : horizontal alignment (only when using resize-mode Crop or Scale)
- **align-y** : _Property (number 0..1)_ : vertical alignment (only when using resize-mode Crop or Scale)
- **play** : _Action_
- **pause** : _Action_
- **stop** : _Action_

## video:snapshot

This component provides the ability to capture and display a still frame from its video input. It extends this ability with the option to fade from the previous captured image to the new one over a period of time, and to mix the new image with the previous one.

- **in** : _VideoIn_
- **out** : _VideoOut_
- **fade-time** : _Property (number 0..60)_ : time in seconds to fade from previous captued image to current captured image
- **mix** : _Property (number 0..1)_ : amount to mix current image on top of previous image, from 0% to 100%
- **trigger** : _Action_ : capture new snapshot
- **reset** : _Action_ : erase current snapshot

## video:still

Output a still image loaded from a file. The image will be drawn onto the input, if the image does not fill the frame or has transparency. Still images are aligned and resized according to the `align-x`, `align-y` and `resize-mode` properties. Images are loaded in the background, and a signal will be sent from the `ready` port when loaded, or the `error` port if an image could not be loaded from the supplied resource location.

- **in** : _VideoIn_
- **out** : _VideoOut_
- **image** : _Property (empty or resource)_ : image (png or jpeg) file location, or empty to clear
- **resize-mode** : _Property (Crop, Stretch, Scale)_ : control how the video frame size is adapted to the output size
- **align-x** : _Property (number 0..1)_ : horizontal alignment (only when using resize-mode Crop or Scale)
- **align-y** : _Property (number 0..1)_ : vertical alignment (only when using resize-mode Crop or Scale)
- **ready** : _ControlOut_
- **error** : _ControlOut_

## video:xfader

Cross fade between two video signals, using the blend mode set. When `mix` is set to 0 or 1 processing of the unused channel will be switched off.

Available blend modes are Normal, Add, Difference, BitXor

- **in-1** : _VideoIn_
- **in-2** : _VideoIn_
- **out** : _VideoOut_
- **mode** : _Property_ : blend mode from above list
- **mix** : _Property (number 0..1)_

## video:analysis:difference

The difference of two video inputs. Supports three modes - Color, Mono and Threshold (black or white). Differences less than the `threshold` value are ignored (black or transparent) in all modes.

This component does not currently support OpenGL acceleration.

- **in-1** : _VideoIn_
- **in-2** : _VideoIn_
- **out** : _VideoOut_
- **mode** : _Property_ : mode from above list
- **threshold** : _Property (number 0..1)_ : ignore differences below this level

## video:analysis:frame-delay

Delay the output by a single frame. Useful for motion analysis, etc.

- **in** : _VideoIn_
- **out** : _VideoOut_

## video:analysis:simple-tracker

A simple blob tracker for motion tracking. Supports tracking of a single blob in the image. 

This component requires the input source to be processed to be useful. See the _Audio/02 blob theremin_ example for one possible way to set this up.

- **in-1** : _VideoIn_
- **in-2** : _VideoIn_
- **debug** : _Property (boolean)_ : draw blobs on screen for debugging purposes
- **x** : _ControlOut (number 0..1)_ : horizontal position of blob centre (normalized)
- **y** : _ControlOut (number 0..1)_ : vertical position of blob centre (normalized)
- **width** : _ControlOut (number 0..1)_ : blob width (normalized)
- **height** : _ControlOut (number 0..1)_ : blob height (normalized)

## video:container:input

Video input for use inside a `core:container`

- **out** : _VideoOut_

## video:container:output

Video output for use inside a `core:container`

- **in** : _VideoOut_

## video:fx:blur

Simple box blur.

This component does not currently support OpenGL acceleration.

- **in** : _VideoIn_
- **out** : _VideoOut_
- **radius** : _Property (number 0..64)_ : blur radius in pixels

## video:fx:ripple

Simple ripple effect. The `disturbance` port controls the amount of rippling of the input source.

This component does not currently support OpenGL acceleration.

- **in** : _VideoIn_
- **disturbance** : _VideoIn_
- **out** : _VideoOut_

## video:gl:filter

Component supporting custom GLSL shaders.

Generally it is better to use `video:gl:p3d`. See the OpenGL components in the custom components library for examples and a base component.

Can only be used with the OpenGL renderer.

- **in** : _VideoIn_
- **out** : _VideoOut_
- **vertex** : _Property (empty or string)_ : vertex shader code, or empty for default
- **fragment** : _Property (empty or string)_ : fragment shader code, or empty for default
- **u1** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u1`
- **u2** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u2`
- **u3** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u3`
- **u4** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u4`
- **u5** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u5`
- **u6** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u6`
- **u7** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u7`
- **u8** : _Property (number 0..1)_ : uniform value which can be read in shader using `uniform float u8`

## video:gl:p2d

Base for video components with almost complete access to the Processing P2D renderer. See coding guide and examples / custom components library.

Can only be used with the OpenGL renderer.

- **out** : _VideoOut_

## video:gl:p3d

Base for video components with almost complete access to the Processing P3D renderer. See coding guide and examples / custom components library.

Can only be used with the OpenGL renderer.

- **out** : _VideoOut_

## video:source:noise

Simple source of white noise.

- **out** : _VideoOut_
