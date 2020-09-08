# Properties & animation

The `Property` class is used for creating property controls on components. It can be used
directly as a field type, and is also the underlying mechanism for other field types such as
`int`, `double`, `String` and `boolean`. Properties can be created using the
[`@P`](coding-annotations.md#p) or  [`@Inject`](coding-annotations.md#inject) annotations.

Properties have some important extra features. Values are automatically saved (unless specified)
as part of the component state. Properties with the same name maintain their value across code 
changes, unlike ordinary fields. 

[Additional annotations](coding-annotations-extra.md) can be used to provide additional
information about a property. This can include its type, range, allowed values, default value, etc.
There are also annotations to mark a property as read-only or transient (value not saved),
to control whether a port is created, or to call a method when the value changes.

Used as the field type `Property` also supports animation with multiple keyframes and easing
modes. Animations also maintain their state across code changes. The `Property` class
also supports [Linkables](coding-linkables.md) for reacting and linking changing values.

## Reading and writing values

The `Property` object itself supports various methods for setting and retrieving the value.
Methods that set the value return the `Property` for efficient method chaining. Get methods
provide an optional default to return if the value cannot be coerced to the required type.

```java
Property set(Argument value);
Property set(double value);

Argument get();
double getDouble(double def);
double getDouble(); // equivalent to getDouble(0)
int getInt(int def);
int getInt(); // equivalent to getInt(0)
boolean getBoolean(boolean def);
boolean getBoolean(); // equivalent to getBoolean(false)
```

There are also some shorthand functions for working with properties.

```java
Property prop;

double d = d(prop); // shorthand for prop.getDouble()
float f = f(prop); // shorthand for (float) prop.getDouble()
int i = i(prop); // shorthand for prop.getInt()
String s = s(prop); // shorthand for prop.get().toString()
boolean b = b(prop); // shorthand for prop.getBoolean()
```

## Animation

`Property` supports animation through a `Property.Animator` object, with multiple keyframes
 and a variety of easing modes available. Animations will continue running even when code is
changed - use the `isAnimating()` to only start an animation if one is not already running.

### Methods on Property

```java
Property.Animator animator();
boolean isAnimating();
Property.Animator to(double ... values);
```

Property.Animator objects are created lazily when required. `isAnimating()` is equivalent
to `animator != null && animator().isAnimating()` so will not create an animator unless
required.

The `to()` method is a shorthand for calling `animator().to()`.

!!! note
    Calling `set(...)` will stop any animation.

### Methods on Property.Animator

Most methods on `Property.Animator` return the animator for fluid method chaining.

```java
Property.Animator to(double ... values);
Property.Animator in(double ... seconds);
Property.Animator easing(Easing ... easings);
Property.Animator stop();

Property.Animator whenDone(Consumer<Property> action);

Property.Animator linear(); // shorthand for easing(Easing.linear)
Property.Animator ease(); shorthand for easing(Easing.ease)
Property.Animator easeIn(); shorthand for easing(Easing.easeIn)
Property.Animator easeOut(); shorthand for easing(Easing.easeOut)
Property.Animator easeInOut(); shorthand for easing(Easing.easeInOut)
```

The number of keyframes is defined by the number of values given to `to()`. You can provide fewer values to `in()` or `easing()` and their values will be cycled. eg. `to(1, 0, 0.5).in(5, 2.5);` will result in -

 - animate from current value to 1 in 5 seconds
 - animate from 1 to 0 in 2.5 seconds
 - animate from 0 to 0.5 in 5 seconds

The `whenDone(...)` method will run an action whenever animation is complete,
and also run the action if not currently animating. It's main use is creating continuous
animations. Restarting the animation within a `whenDone` handler will cause the
property to remove any overrun of time from the first keyframe for accurate time handling.



### Easing modes

The various easing modes and their definitions are listed here - 

```java
public final static Easing linear = new LinearEasing();
public final static Easing easeIn = new SplineEasing(0.42, 0, 1, 1);
public final static Easing easeOut = new SplineEasing(0, 0, 0.58, 1);
public final static Easing easeInOut = new SplineEasing(0.42, 0, 0.58, 1);
public final static Easing ease = new SplineEasing(0.25, 0.1, 0.25, 1);
public final static Easing expoIn = new SplineEasing(0.71,0.01,0.83,0);
public final static Easing expoOut = new SplineEasing(0.14,1,0.32,0.99);
public final static Easing expoInOut = new SplineEasing(0.85,0,0.15,1);
public final static Easing circIn = new SplineEasing(0.34,0,0.96,0.23);
public final static Easing circOut = new SplineEasing(0,0.5,0.37,0.98);
public final static Easing circInOut = new SplineEasing(0.88,0.1,0.12,0.9);
public final static Easing sineIn = new SplineEasing(0.22,0.04,0.36,0);
public final static Easing sineOut = new SplineEasing(0.04,0,0.5,1);
public final static Easing sineInOut = new SplineEasing(0.37,0.01,0.63,1);
public final static Easing quadIn = new SplineEasing(0.14,0.01,0.49,0);
public final static Easing quadOut = new SplineEasing(0.01,0,0.43,1);
public final static Easing quadInOut = new SplineEasing(0.47,0.04,0.53,0.96);
public final static Easing cubicIn = new SplineEasing(0.35,0,0.65,0);
public final static Easing cubicOut = new SplineEasing(0.09,0.25,0.24,1);
public final static Easing cubicInOut = new SplineEasing(0.66,0,0.34,1);
public final static Easing quartIn = new SplineEasing(0.69,0,0.76,0.17);
public final static Easing quartOut = new SplineEasing(0.26,0.96,0.44,1);
public final static Easing quartInOut = new SplineEasing(0.76,0,0.24,1);
public final static Easing quintIn = new SplineEasing(0.64,0,0.78,0);
public final static Easing quintOut = new SplineEasing(0.22,1,0.35,1);
public final static Easing quintInOut = new SplineEasing(0.9,0,0.1,1);
```

### Examples

This simple example creates a property that counts seconds. It will continue running
through code changes, and restart after 24 hours.

```java
@P(1) Property seconds;

public void update() {
  if (!seconds.isAnimating()) {
    seconds.set(0).to(86400).in(86400);
  }
}
```

This example animates a rectangle from the left to the middle of the screen and back when a trigger is received (eg. button clicked). It use `@Inject` to create a hidden property purely for animation purposes (no port or control, and not saved).

```java
@Inject Property x;

public void draw() {
  rect(d(x) * width, (height / 2) - 25, 50, 50);
}

@T(1) void move() {
  x.to(0.5, 0).in(2.25).easing(Easing.easeIn, Easing.cubicOut);
}
```

To continuously animate to a random value every 5 seconds using `whenDone`.

```java

@Inject Property x;

public void init() {
  x.animator().whenDone(p -> p.to(random(0, 1).in(5).easeInOut());
}
```