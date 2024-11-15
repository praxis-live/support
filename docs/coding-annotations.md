# Annotations

The primary annotations listed here connect fields and methods in code to the wider
environment by defining ports & controls, or by automatically creating and injecting values.
[Additional annotations](coding-annotations-extra.md) can be used to enhance some of these 
annotations, such as specifying the range of a numeric property or the mime-type of a String.

Fields should not be set when declared - values will be injected automatically.

## @In

```java
@interface In {
  int value();
}
```

Mark a field or method as an input. An input port will be created on the component.
The optional weight value controls the position of the port relative to other input
ports.

Supported on fields of type `Input`, `Data.In`, `AudioIn`, `PImage` and `Ref.Input<T>`.

Supported on void methods taking a single argument of type `double`, `int`, `String`
or a `Value` subclass.

The `Input` class supports reacting to input using [Linkables](coding-linkables.md).

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

Mark a field as an output. An output port will be created on the component. The
optional weight value controls the position of the port relative to other output
ports.

Supported on fields of type `Output`, `Data.Out`, `AudioOut` and `Ref<T>`.

The `Output` class supports methods for sending various types, including `double`,
`String` and `Value` subclasses, as well as an empty message.

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

Mark a field as a property. A control will be added to the component, and (by default)
an input port. The optional weight value controls the position of the property relative
to other properties.

Supported on core fields of type `Property`, `boolean`, `double`, `int`, `String`,
all `Value` types and `Optional` of all value types.

Audio components also support use on fields of type `AudioTable`. Video components also
support use on fields of type `PImage`.

Fields of most types may be written to as well as read.

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

## @T

```java
@interface T {
  int value();
}
```

Mark a field or method as a trigger (action). A control will be added to the component,
and (by default) an input port. The value specifies the index relative to other triggers - all
triggers must have a unique index.

Supported on fields of type `boolean` and `Trigger`, and zero-argument void methods.

A `boolean` field will be set to `true` when triggered - it should be explicitly set back to `false`.

The `Trigger` type supports [Linkables](coding-linkables.md) as well as attaching `Runnable`.

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

## @FN

Annotate a method as a custom function control. Supports arguments and return values of
any `Value` subtype, as well as `String`, `boolean`, `int`, `float` and `double`.

### Examples

```java
@P @Type.String(def = "Hello") String greeting;

@FN String greet(String name) {
    return greeting + " " + name;
}
```

## @AuxIn

```java
@interface AuxIn {
  int value();
}
```

Mark a field or method as an auxillary input. Works almost identically to `@In` except
that ports appear after input, output, property and trigger ports.

## @AuxOut

```java
@interface AuxOut {
  int value();
}
```

Mark a field or method as an auxillary output. Works almost identically to `@Out` except
that ports appear after input, output, property, trigger and auxillary input ports.

## @Inject

```java
@interface Inject
```

Supported on the same core fields as `@P` and acts as a hidden property. If used on `Property`
is useful for animation. Hidden properties do not show up in the editor and their state is not
saved as part of the project. However, unlike normal fields they maintain state across code
changes.

`@Inject` is used for [`Ref<T>`](coding-ref.md) and [`Data.Sink`](coding-data-pipes.md)
fields.

`@Inject` can also be used with fields of other types via [`Ref.Provider`](coding-ref.md).
The default provider supports injecting fields of type `List`, `Set` and `Map`. A custom
provider can be specified using the `provider` value on the annotation.

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

## @Persist

Mark a field to be persisted across code changes. Should be used with care. Unlike injected
fields, persistent fields are not automatically set. Can be used for any type.

Persistent fields are reset to default values (eg. `0` or `null`) when the root is restarted,
unless the `autoReset` value of the annotation is set to `false`. Like `Ref` values, any
`AutoCloseable` values will be closed when removed or reset unless the `autoClose` value of
the annotation is set to `false`.

### Examples

```java
@Persist int counter;
@Persist(autoReset = false) Future<String> result;
```

## @OffScreen

```java
@interface OffScreen {
  int width() default 0;
  int height() default 0;
  double scaleWidth() default 1;
  double scaleHeight() default 1;
  boolean persistent() default true;
  OffScreen.Format format() default OffScreen.Format.Default; // also RGB and ARGB
}
```

Mark a `PGraphics` field to create an offscreen surface for rendering. Supported by `video:custom`,
`video:gl:p2d` and `video:gl:p3d`. See [Coding video components](coding-video.md) or
[Coding OpenGL video components](coding-video-gl.md) for more information.

## @UGen

```java
@interface UGen
```

Mark a field as an audio unit generator. Only supported on fields subclassing `Pipe` in
`audio:custom`. See [Coding audio components](coding-audio.md) for more information.
