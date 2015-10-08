# Coding in Praxis LIVE

If you can't find a built-in or existing components that does what you want, it's easy to get coding in _Praxis LIVE_. The code of _Praxis LIVE_ components is based on the excellent and easy to use [Processing](https://processing.org/) project. Not only that, but you can rewrite components _on-the-fly_, while your project is running.

To get started, choose one of the base components and drag it into your graph. Alternatively, choose a built-in component or custom component that does some of what you want and start from there instead. Right-click on the component in the graph window and select `Edit code` to open the code in the code editor. Everytime you hit `Save` within the code editor your code will be recompiled and inserted into your component, even as it's running. The code editor supports code completion and will highlight errors in your code.

**Note that saving code in the code editor saves the code to the component, it does not save it to disk - make sure you also save your project!**

If you're used to working with Processing, have a [read of this blog post](https://praxisintermedia.wordpress.com/2015/04/01/transform-a-processing-sketch-into-a-praxis-live-component/) which covers some of the differences when working with Processing code inside _Praxis LIVE_.

The base types for different component types are listed here. Also see the annotation sections for details of how to hook up code to ports and controls.

## core:custom

The base type for components that work with control signals inside any graph type.

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage.
 - `update()` : called every update cycle (audio buffer, video frame, etc.). If your component reacts solely to input and doesn't need to be called every cycle, remove this method for efficiency.
 - `starting()` : called after setup whenever the pipeline is started (not called on code updates).

You have access to most of the Processing API not related to graphical operations. This does not include functions not suitable for real-time operation (would normally be advised only in setup)

## audio:custom

The base type for audio components.

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage.
 - `update()` : called every update cycle before the next audio buffer is processed. If your component reacts solely to input and this method would otherwise be empty, remove it for efficiency.

You have access to most of the Processing API not related to graphical operations, and a range of audio unit generators (UGens) and audio related functions (more info coming soon - see custom audio components for examples)

## video:custom

The base type for video components.

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage. Unlike other components, setup is called in the render cycle so like in Processing you can draw to the output.
 - `draw()` : called every render cycle. Draw to the output.
 
As well as access to the Processing API as supported by `core:custom` you have access to a limited subset of the graphics API. If you need full access to the power of the Processing graphics API, look at `video:gl:p3d` instead.

## video:gl:p3d

Video base type providing almost complete access to the Processing 3D API. Only supported in the OpenGL renderer.

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage. Unlike other components, setup is called in the render cycle so like in Processing you can draw to the output.
 - `draw()` : called every render cycle. Draw to the output.

## tinkerforge:custom

The base type for TinkerForge components. A single Brick or Bricklet device field annotated with `@TinkerForge` will be automatically injected based on the field type and the `uid` property of the component. The component ensures that all TinkerForge callback are called in the right thread so that you can safely send messages in device listeners.

The primary method hooks you can override are -

 - `setup()` : called whenever a brick / bricklet connection is made (field injected) and any time the code is updated.
 - `update()` : called on every update cycle _when a brick / bricklet is connected_.
 - `dispose()` : called when the component is about to be disconnected from the brick / bricklet. Should remove listeners and set callback rates back to 0.


