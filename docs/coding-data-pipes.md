# Data Pipes

The Data Pipes API allows for passing arbitrary types around the node graph. It works
similarly to the existing support for audio and video buffers, and is similar to programming
with the existing audio Pipes API. Ports are typed using generics, and only ports of the same
type may be connected together. The type shows as the port category in the UI.

Unlike control messages, but like audio and video, data is pulled through the graph from a sink.
The `Data.Sink` type cab be provided with functions for controlling what happens when multiple
connections are made to ports. You must pass an object representing the data to the sink process
method - the returned object may be the same or different depending on how you configure the
processing graph.

eg. to provide a list of PVector geometry to draw (NB. you can cache a list of geometry, but
remember that if you change the PVectors inside it, you'll have to reset them - they are not
copied by default!)

## Example

Component 1 (core:custom)

```java
@Out(1) Data.Out<List<PVector>> out;

public void init() {
    Data.link(
        Data.supply(this::geometry),
        out
    );
}

List<PVector> geometry() {
    // generate geom
    return list;
}

```

Component 2 (video:gl:p3d)

```java

@AuxIn(1) Data.In<List<PVector>> geom;

@Inject Data.Sink<List<PVector>> sink;

public void setup() {
    Data.link(geom, sink.input());
}

public void draw() {
    // passing empty list means we'll do nothing if not connected
    // to previous component
    for (PVector v : sink.process(Collections.emptyList()) {
        // do something with v
    }
}
```

In a similar way you can share code from components by passing interfaces, in particular
the Java 8+ functional types can be passed and composed.

