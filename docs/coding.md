# Coding in Praxis LIVE

If you can't find a built-in or existing components that does what you want, it's easy to get coding in _Praxis LIVE_. The code of _Praxis LIVE_ components is based on the excellent and easy to use [Processing](https://processing.org/) project. Not only that, but you can rewrite components _on-the-fly_, while your project is running.

To get started, choose one of the base components and drag it into your graph. Alternatively, choose a built-in component or custom component that does some of what you want and start from there instead. Right-click on the component in the graph window and select `Edit code` to open the code in the code editor. Everytime you hit `Save` within the code editor your code will be recompiled and inserted into your component, even as it's running. The code editor supports code completion and will highlight errors in your code.

**Note that saving code in the code editor saves the code to the component, it does not save it to disk - make sure you also save your project!**

## Base types

The base types for different component types are listed here. Note that most base types build on the functionality of `core:custom`.

 - `core:custom` : Base type for components that work with control signals inside any graph type. See [Coding control components](coding-core.md)
 - `audio:custom` : Base type for audio components. See [Coding audio components](coding-audio.md)
 - `video:custom` : Base type for simple video components. See [Coding video components](coding-video.md)
 - `video:gl:p2d` : Video base type providing almost complete access to the Processing 2D API. Only supported in the OpenGL renderer. See [Coding OpenGL video components](coding-video-gl.md)
 - `video:gl:p3d` : Video base type providing almost complete access to the Processing 3D API. Only supported in the OpenGL renderer. See [Coding OpenGL video components](coding-video-gl.md)
 - `tinkerforge:custom` : Base type for components interfacing with [TinkerForge](http://www.tinkerforge.com) sensors and electronics. See [Coding TinkerForge components](coding-tinkerforge.md)

## Core syntax and functionality

The primary coding language used in _Praxis LIVE_ is Java. If you've not used Processing or Java before, you should probably take some time to look through the Processing [tutorials](https://processing.org/tutorials/) and [reference](https://processing.org/reference/) - much of the information there will be useful to you.

Having said that, don't be scared to play - the built-in code editor in _Praxis LIVE_ will highlight most errors in your code and supports [intelligent code completion](https://en.wikipedia.org/wiki/Intelligent_code_completion).

### Using annotations

Unlike Processing, _Praxis LIVE_ makes a lot of use of annotations on fields and methods. These annotations are used to integrate your code with the environment, such as providing visual ports or controls, automatically saving state, allowing variables to be controlled by MIDI / GUI, provide background loading of resources, etc.

```java
@In(1) PImage in;

@P(1) @Port(false) PImage image;
@P(2) @Type.Number(min = 0, max = 1) scale;

@T(1) void randomize() {
  scale = random(1);
}

public void draw() {
  // do something with image and scale
}
```

The above code is based on one of the video base components. The above code will provide a video input port, an image property (without port) that supports background loading of an image file, a numeric scale property with visual slider, and a trigger control with visual button that calls the annotated method. Note that none of the fields are initialized by the code - all annotated fields will be automatically set by the environment.

For more information see the [documentation for individual annotations](annotations.md).

### Properties and Triggers

All base component types support the use of the [`@P`](annotations.md#p) and [`@T`](annotations.md#t) annotations to define properties and triggers (actions).

The core fields types supported by the property annotation are `double`, `int`, `String`, `boolean` and `Property`. All the core field types are backed by a `Property` object, which also supports animation - see the full [Property documentation](properties.md). Base types may extend the fields that can be marked with the property annotation (eg. video base types support PImage fields).

Values of property fields will normally be saved as part of the project file, unless the property is marked with [`@Transient`](annotations-additional.md#transient) or [`@ReadOnly`](annotations-additional.md#readonly). The new `@Inject` annotation also supports properties that are hidden and transient - useful for maintaining state or animation while coding.

The trigger annotation can be placed on a zero argument method, or on a field of type `boolean` or `Trigger`. If using a `boolean` field, the value will be set to `true` when triggered - _you are responsible for resetting it_.

### Input and Output

All base components support the use of annotations to provide ports for control signal input and output. The auxillary annotations cause the ports to appear after property and trigger ports rather than before.

#### @In / @AuxIn

Use the [`@In`](annotations.md#in) or [`@AuxIn`](annotations.md#auxin) annotations on a method to provide a port with which to receive control values -

```java
@In(1) void in(double input) {
  // do something with input
}
```

The method will be called at the correct time with the input value. Supported types are `double`, `int`, `String` and `Argument`.

_Be aware that input methods will be called during the update rather than rendering part of the video processing cycle - if using in a video component, you cannot call drawing functions._

#### @Out / @AuxOut

Use the [`@Out`](annotations.md#out) or [`@AuxOut`](annotations.md#auxout) annotation on a field of type `Output` to provide control signal output ports on your component.

The `Output` object supports methods for sending `double`, `String` and `Argument`, as well as an empty signal.

```java
@Out(1) Output out;

@In(1) void in(double input) {
  out.send(input);
}
```

_Output sending should only happen during the update part of the video rendering cycle, which means that currently you should not call `Output.send(..)` during the `draw()` method of a video component._

### Coming from Processing?

If you're used to working with Processing, have a [read of this blog post](https://praxisintermedia.wordpress.com/2015/04/01/transform-a-processing-sketch-into-a-praxis-live-component/) which covers some of the differences when working with Processing code inside _Praxis LIVE_.

