# Linkables

Linkables are a different way of responding to actions (triggers), input and property
changes, based on a simplified form of reactive stream. There is a `Linkable<T>` generic
type, plus primitive versions `Linkable.Int` and `Linkable.Double`. All support `filter()`
and `map()` operations. At the end of the chain a consumer is passed to `link()` to handle
the resulting value.

## Built-in sources

Linkables are provided by the built in `Input`, `Trigger` and `Property` types. All links
are cleared before `init()` / `setup()` is called, so in usual usage you do not need to 
care about disconnection.

### Input

```java
// Linkable of primitive doubles
Linkable.Double values();
// Linkable of subtypes of Value
<T extends Value> Linkable<T> valuesAs(Class<T> valueType);
// Linkable of any type converted from Value
<T> Linkable<T> valuesAs(Function<Value, T> converter);
```

### Property

```java
// Linkable of primitive doubles
Linkable.Double values();
// Linkable of subtypes of Value
<T extends Value> Linkable<T> valuesAs(Class<T> valueType);
// Linkable of any type converted from Value
<T> Linkable<T> valuesAs(Function<Value, T> converter);
```

Property also has direct `link(...)` and `linkAs(...)` methods where mapping and/or
filtering is not required.

### Trigger

```java
// receives a rising count on each trigger
// wraps to zero at Integer.MAX_VALUE unless maxIndex() set
Linkable.Int on();
```

Trigger also has a `link(Runnable action)` method.

## Examples

A simple component to uppercase all received input.

```java
@In(1) Input in;
@Out(1) Output out;

public void init() {
    in.valuesAs(Value::toString)
      .map(String::toUpperCase)
      .link(out::send);
}
```

A component reacting to audio clock and level property.

```java
@T(1) Trigger beat;
@P(1) Property level;
@Out(1) Output note;
@Out(2) Output gain;

public void init() {
  beat.maxIndex(16);
  beat.on()
      .filter(i -> i % 4 == 0)
      .link(i -> note.send(randomOf("a3", "d3", "e3"));
  level.values()
      .map(d -> d * d * d * d)
      .link(gain::send);
}
```
