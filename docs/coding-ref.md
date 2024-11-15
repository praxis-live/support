# Custom references

Most of the time working with _PraxisCORE_ you do not need to manage references to objects manually.
However, `Ref<T>` offers a way to hook into the reference handling lifecycle inside the runtime
when this is required eg. for interacting with other libraries. It provides a safe way
for carrying references across code changes, and determining what happens when code is updated
or the reference deleted.

The reference must be initialized before use, often with a constructor references. Handlers can be
added for `onReset()` and `onDispose()` - the former will be called every code change, the latter
when the root is stopped or the reference deleted. If the reference is `AutoCloseable` then it will
be closed automatically on disposal by default.

Use eg.

```java
@Inject Ref<List<String>> strings;

public void init() {
    strings.init(ArrayList::new)
           .apply(l -> {
                if (l.isEmpty()) {
                    l.add("A String");
                }
            });
    List<String> lst = strings.get(); // same list across code changes
}
```

See the Javadoc for `Ref<T>` included with the IDE for the full range of features.

## Ref.Provider

As well as using `Ref<T>` directly in components, a reference provider can be registered to
handle the configuration of multiple references from one place. The `@Inject` annotation
supports a `provider` value. The provider implementation can be a static class or separately
in [shared code](coding-shared.md) to manage references across a graph. The field itself
should be of the injected type.

There is also a default reference provider, that allows injecting fields of `List`, `Set`
and `Map` which will inject `ArrayList`, `LinkedHashSet` and `LinkedHashMap` respectively.

```java
package SHARED;
// imports

public class Types extends Ref.Provider {
    public Types() {
        register(List.class, (Ref<List<?>> r) ->
                r.init(LinkedList::new).onReset(List::clear));
    }
}
```

```java
@Inject List<String> arrayList;
@Inject(provider = SHARED.Types.class) List<String> linkedList;
```

## Reference ports

A `Ref<T>` field can be annotated with `@Out` to create a reference output port. Input
ports use `Ref.Input<T>`, which provides a `List<T>` of values from connected references.

```java
@Out Ref<String> out;
```

```java
@In Ref.Input<String> in;

public void init() {
    List<String> values = in.values();
    in.onUpdate(list -> {
        if (!list.isEmpty()) {
            // do something with inputs
        }
    });
}
```

## Publish and Subscribe

A `Ref<T>` field in a container can be marked with `@Publish` annotation. Any `Ref<T>` fields
in direct child components can be annotated with `@Subscribe` and will automatically update
in sync with the container value. Both annotations support an optional `name`.

```java
@Inject @Ref.Publish
Ref<String> defRef;

@Inject @Ref.Publish(name = "alt")
Ref<String> altRef;

public void init() {
    defRef.set("default");
    altRef.set("alternate");
}
```

```java
@Inject @Ref.Subscribe
Ref<String> defRef;

@Inject @Ref.Subscribe(name = "alt")
Ref<String> altRef;

public void init() {
    String foo = altRef.orElse("");
    // foo == "alternate"
}
```

## Async set

It's possible to set the value of a `Ref<T>` field using an `Async` task. Care should be
taken not to capture state of the component in any background task.
 