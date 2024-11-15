# Coding in PraxisLIVE

_PraxisLIVE_ is a code-first visual environment, in the sense that the code for almost
every component is editable and can be adapted, even as it's running. If you can't find
a built-in or existing custom component that does what you want, it's easy to adapt one
or build one from scratch.

To get started, choose one of the base components and drag it into your graph. Alternatively,
choose a built-in component or custom component that does some of what you want and start from
there instead. Right-click on the component in the graph window and select `Edit code` to open
the code in the code editor. Every time you hit `Save` within the code editor your code will be
recompiled and inserted into your component, even as it's running. 

!!! info
    Saving code in the code editor updates the code of the component, it does
    not save everything to disk - make sure you also save your project!

## Base types

The base types for different component types are listed here. Note that most base types
build on the functionality of `core:custom`.

 - `core:custom` : Base type for components that work with control signals inside any
graph type. See [Coding control components](coding-core.md)
 - `data:custom` : Base type for components that work with control signals inside a
data root. This base type should be used for components that are not real-time safe and
so should not be added to other graph types. See [Coding control components](coding-core.md)
 - `audio:custom` : Base type for audio components. See [Coding audio components](coding-audio.md)
 - `video:custom` : Base type for simple video components. See [Coding video components](coding-video.md)
 - `video:gl:p2d` : Video base type providing almost complete access to the Processing 2D API.
Only supported in the OpenGL renderer. See [Coding OpenGL video components](coding-video-gl.md)
 - `video:gl:p3d` : Video base type providing almost complete access to the Processing 3D API.
Only supported in the OpenGL renderer. See [Coding OpenGL video components](coding-video-gl.md)

## Core syntax and functionality

The primary coding language used in _PraxisLIVE_ is Java, although much should be familiar
if you've used Processing before.

The code editor supports full code completion and will highlight any errors in your code. Even if
you're new to coding, don't be afraid to experiment! You can always use undo or reset the code
to the component default or last saved state.

### Using annotations

_PraxisCORE_ uses annotations on fields and methods to integrate your code with the environment,
such as providing ports and controls, automatically saving state, allowing variables to be controlled
from elsewhere, provide background loading of resources, etc.

```java
@In(1) PImage in;

@P(1) @Config.Port(false) PImage image;
@P(2) @Type.Number(min = 0, max = 1) scale;

@T(1) void randomize() {
  scale = random(1);
}

public void draw() {
  // do something with image and scale
}
```

The above code is based on one of the video base components. It will provide a video input port, an
image property (without port) that supports background loading of an image file, a numeric scale property and a
trigger control that calls the annotated method. The graph editor in _PraxisLIVE_ will use this information to
show a node with file browser for image, a slider for the ranged scale, and a button for randomize.

Note that none of the fields are initialized by the code - all annotated fields will be automatically set by
the environment, carried over from one code iteration to the next.

For more information see the [documentation for individual annotations](coding-annotations.md).

### Properties and Triggers

All base component types support the use of the [`@P`](coding-annotations.md#p) and [`@T`](coding-annotations.md#t)
annotations to define properties and triggers (actions).

The core fields types supported by the property annotation are `double`, `int`, `String`, `boolean`, as well
as `Property` and any `Value` subclass. Property also supports animation - see the full 
[Property documentation](coding-properties.md). Base types may extend the fields that can be marked
with the property annotation (eg. video base types support `PImage` fields).

Values of property fields will normally be saved as part of the project file, unless
the property is marked with [`@Transient`](coding-annotations-extra.md#transient) or
[`@ReadOnly`](coding-annotations-extra.md#readonly). The `@Inject` annotation also supports
properties that are hidden and transient - useful for maintaining state or animation while coding.

The trigger annotation can be placed on a zero argument method, or on a field of type `boolean`
or `Trigger`. If using a `boolean` field, the value will be set to `true` when triggered
- _you are responsible for resetting it_.

### Input and Output

All base components support the use of annotations to provide ports for control
signal input and output. The auxillary annotations cause the ports to appear after
property and trigger ports rather than before.

#### @In / @AuxIn

Use the [`@In`](coding-annotations.md#in) or [`@AuxIn`](coding-annotations.md#auxin) annotations on a
method to provide a port with which to receive control values -

```java
@In(1) void in(double input) {
  // do something with input
}
```

The method will be called at the correct time with the input value. Supported types are
`double`, `int`, `String` and and `Value` subclass.

#### @Out / @AuxOut

Use the [`@Out`](coding-annotations.md#out) or [`@AuxOut`](coding-annotations.md#auxout) annotation
on a field of type `Output` to provide control signal output ports on your component.

The `Output` object supports methods for sending `double`, `String` and `Value` subclasses,
as well as an empty signal.

```java
@Out(1) Output out;

@In(1) void in(double input) {
  out.send(input);
}
```
