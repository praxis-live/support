# Coding control components

`core:custom` is the base type for components that work with control signals within
any graph type. You have access to most of the Processing API not related to graphical
operations. This does not include functions not suitable for real-time operation.

## Key method hooks

The key method hooks you can override inside a `core:custom` component are -

 - `init()` : called each time the pipeline is started and any time the code is updated.
Because this method is called on code updates, it needs to be suitable for real-time usage.
 - `update()` : called every update cycle (audio buffer, video frame, etc.). _If your
component reacts solely to input and doesn't need to be called every cycle, remove
this method for efficiency._
 - `starting()` : called after init whenever the pipeline is started (not called on code updates).

## Input

Use the [`@In`](annotations.md#in) annotation on a method to provide a port with which to receive control values -

```java
@In(1) void in(double input) {
  // do something with input
}
```

The method will be automatically be called at the correct time with the input value.
Supported types are `double`, `int`, `String` and `Value` subclasses.

You can also receive input using the [`@T`](annotations.md#t) annotation on a method,
or using the [`@OnChange`](annotations-additional.md#onchange) annotation to define a
method to call when a property changes value.

## Output

Use the [`@Out`](annotations.md#out) annotation on a field of type `Output` to provide
control signal output ports on your component.

The `Output` object supports methods for sending `double`, `String` and `Value` subclasses,
as well as an empty signal.

```java
@Out(1) Output out;

@In(1) void in(double input) {
  out.send(input);
}
```
