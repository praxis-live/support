# Annotations

The primary annotations listed here connect fields and methods in code to the wider _PraxisLIVE_ environment by defining ports & controls, or by automatically creating and injecting values. [Additional annotations](annotations-additional.md) can be used to enhance some of these annotations, such as specifying the range of a numeric property or the mime-type of a String.

Fields should not be set when declared - values will be injected automatically by the environment.

## @In

```java
@interface In {
  int value();
}
```

Mark a field or method as an input. An input port will be created on the component. The value specifies the index of the port relative to other input ports - all input ports must have a unique index.

Supported on fields of type `PImage`.

Supported on void methods taking a single argument of type `double`, `int`, `String` or `Argument`.

**Inside a video component, the method input will be called during the update rather than render cycle - accessing graphics methods will fail.**

### Examples

```java
@In(1) PImage in;
```

```java
@In(2) void in2(double value) {
  if (value > 0.5) {
    // do something
  }
}
```

## @Out

```java
@interface Out {
  int value();
}
```

Mark a field as an output. An output port will be created on the component. The value specifies the index of the port relative to other output ports - all output ports must have a unique index.

Only supported on fields of type `Output`. The Output object supports methods for sending various types, including `double`, `String` and `Argument`, as well as an empty message.

**In a video component, the Output object should not be used within the render cycle - inside `setup()` or `draw()`.**

### Examples

```java
@Out(1) Output out;

void update() {
  if (someCondition) {
    out.send();
  }
}
```

```java
@Out(1) Output out;

@In(1) void in(double value) {
  if (value > 0.5) {
    out.send(value);
  }
}
```

## @P

```java
@interface P {
  int value();
}
```

Mark a field as a property. A control will be added to the component, and (by default) an input port. The value specifies the index of the property relative to other properties - all properties must have a unique index.

Supported on core fields of type `boolean`, `double`, `int`, `String` and `Property`. Audio components also support use on fields of type `Table`. Video components also support use on fields of type `PImage`.

Fields may be written to as well as read. All property controls are actually backed by a Property object, which offers additional features such as animation. Some properties (eg. `PImage`) support background loading of a resource from a file.

<s>Properties may be hidden from the editor by prefixing their name with an underscore - particularly useful for animation. Hidden properties still maintain state across code changes.</s> Deprecated - see [`@Inject`](#inject)

### Examples

```java
@P(1) double x;
```

```java
@P(2) Property rotation;

void draw() {
  if (!rotation.isAnimating()) {
    rotation.to(random(0,360)).in(4).ease();
  }
  rotate(radians(d(rotation)));
  // draw something
}
```

```java
@P(3) PImage img;

void draw() {
  if (img != null) {
    image(img, 50, 50);
  }
}
```

## @Inject

```java
@interface Inject
```

Mark a field as a hidden property, particularly useful for animation. Hidden properties do not show up in the editor and their state is not saved as part of the project. However, unlike normal fields they maintain state across code changes.

Supported on core fields of type `boolean`, `double`, `int`, `String` and `Property`.

Unlike `@P`, the `@Inject` annotation does not require an index and so multiple fields of the same type may be declared together.

### Examples

```java
@Inject double x, y;
```

```java
@Inject Property timer;

void update() {
  if (!timer.isAnimating()) {
    timer.set(0).to(60).in(60);
  }
}
```

## @T

```java
@interface T {
  int value();
}
```

Mark a field or method as a trigger (or action). A control will be added to the component, and (by default) an input port. The value specifies the index of the port relative to other trigger ports - all trigger ports must have a unique index.

Supported on fields of type `boolean` and `Trigger`, and zero-argument void methods.

A `boolean` field will be set to `true` when triggered - it should be explicitly set back to `false`. The Trigger object supports two methods that return `boolean` - `peek()` and `poll()` - the former of which does not reset the trigger.

**Inside a video component, the method version of a trigger will be called during the update rather than render cycle - accessing graphics methods will fail.**

### Examples

```java
@T(1) boolean reset;

void draw() {
  if (reset) {
    // reset render settings
    reset = false;
  }
  // render
}
```

```java
@Out(1) Output out;
@P(1) double value;

@T(1) void trigger() {
  out.send(value);
}
```

## @AuxIn

```java
@interface AuxIn {
  int value();
}
```

Mark a field or method as an auxillary input. Works almost identically to `@In` except that ports appear after input, output, property and trigger ports.

## @AuxOut

```java
@interface AuxOut {
  int value();
}
```

Mark a field or method as an auxillary output. Works almost identically to `@Out` except that ports appear after input, output, property, trigger and auxillary input ports.

## @OffScreen

```java
@interface OffScreen {
  int width() default 0;
  int height() default 0;
  OffScreen.Format format() default OffScreen.Format.Default; // also RGB and ARGB
}
```

Mark a `PGraphics` field to create an offscreen surface for rendering. Only supported by `video:custom`. See [Coding video components](coding-video.md) for more information.

## @UGen

```java
@interface UGen
```

Mark a field as an audio unit generator. Only supported on fields subclassing `Pipe` in `audio:custom`. See [Coding audio components](coding-audio.md) for more information.
