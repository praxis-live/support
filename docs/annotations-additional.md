# Additional annotations

These additional annotations can be used alongside [primary annotations](annotations.md) to provide additional configuration for a field or method.

## @ID

```java
@interface ID {
  String value();
}
```

Override the default ID for a port or control.

### Example

```java
// default id would be screen-width
@P(1) @ID("width") int screenWidth; 
```

## @OnChange

```java
@interface OnChange {
  String value();
}
```

Specify a method to run when a property value changes. Especially useful for knowing when a background loading resource is ready. The method should take no arguments and return `void`.

**Inside a video component, the method will be called during the update rather than render cycle - accessing graphics methods will fail.**

### Example

```java
@P(1) @OnChange("reconfigure") double scale;

void reconfigure() {
  // do something when scale changes
}
```

## @OnError

```java
@interface OnError {
  String value();
}
```

Specify a method to run when setting a property fails. Especially useful for situations where background loading a resource fails. The method should take no arguments and return `void`.

**Inside a video component, the method will be called during the update rather than render cycle - accessing graphics methods will fail.**

### Example

```java
@P(1) @OnError("imgError") PImage img;
@AuxOut(2) Output error;

void imgError() {
  error.send();
}
```

## @Port

```java
@interface Port {
  boolean value();
}
```

Control the creation of a port. Currently only used to suppress the creation of ports for properties and triggers.

### Examples

```java
@P(1) @Port(false) PImage img;
```

```java
@T(1) @Port(false) void reset() {
  // reset stuff
}
```

## @ReadOnly

```java
@interface ReadOnly {}
```

Specify a property is read only. No port will be created. Attempts to set the property via its control will fail. The variable can still be set in code and read via the control.

### Example

```java
@P(5) @ReadOnly int length;
```

## @Transient

```java
@interface Transient {}
```

Mark a property as transient, meaning the current value will not be saved in the project. Useful for continuously changing values (eg. media position)

### Example

```java
@P(2) @Type.Number(min = 0, max = 1) @Transient
double position;
```

## @Type

```java
@interface Type {
  Class<? extends Argument> cls() default Argument.class;
}
```

Specify a property as any subclass of Argument.

### Example

```java
@P(4) @Type(cls = ControlAddress.class) Property to;
```

## @Type.Boolean

```java
@interface Type.Boolean {
  boolean def() default false;
}
```

Specify a property is a boolean, with optional default value.

### Example

```java
@P(4) @Type.Boolean(def = true) render;
```

## @Type.Integer

```java
@interface Type.Integer {
  int min() default PNumber.MIN_VALUE;
  int max() default PNumber.MAX_VALUE;
  int def() default 0;
}
```

Specify a property is an integer with optional range and default value.

### Example

```java
@P(2) @Type.Integer(min = 1, max = 10, def = 1)
int count;
```

## @Type.Number

```java
@interface Type.Number {
  double min() default PNumber.MIN_VALUE;
  double max() default PNumber.MAX_VALUE;
  double def() default 0;
}
```

Specify a property is numeric (`double`) with optional range and default value. Ranged numeric properties will show as a slider in the property editor.

### Examples

```java
@P(1) @Type.Number(min = 0, max = 360) Property rotation;
```

```java
@P(2) @Type.Number(min = 0, max = 1, def = 0.5) double position;
```

## @Type.String

```java
@interface Type.String {
  String[] allowed() default {};
  boolean emptyIsDefault() default false;
  String mime() default "";
  String def() default "";
  String template() default "";
}
```

Specify a property is a string, with optional default value. The emptyIsDefault parameter will cause the word `default` to show in the editor when the value is an empty string.

If the allowed array is not empty, the property will act like an enumeration, providing a select box in the editor.

The mime type may be set to control editing / syntax highlighting, and the template may be used to specify text to use in the editor when the value is empty.

### Examples

```java
@P(1) @Type.String(allowed = {"Blend", "Add", "Multiply"})
String blendMode;
```

```java
@P(1) @Type.String(mime = GLSL_FRAGMENT_MIME, template = DEFAULT_FRAGMENT_SHADER)
String fragment;
```


