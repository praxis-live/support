# Ref&lt;T&gt;

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

public void setup() {
    strings.init(ArrayList::new);
    strings.apply(l -> {
        if (l.isEmpty()) {
            l.add("A String");
        }
    });
    List<String> lst = strings.get(); // same list across code changes
}
```

See the Javadoc for `Ref<T>` included with the IDE for the full range of features.