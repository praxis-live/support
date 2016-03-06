# Coding TinkerForge components

`tinkerforge:custom` is the base type for creating components that interact with the [TinkerForge](http://www.tinkerforge.com) range of electronics building blocks.

A _single_ Brick or Bricklet device field annotated with `@TinkerForge` will be automatically injected based on the field type and the `uid` property of the component. The component ensures that all TinkerForge callbacks are called in the right thread so that you can safely send messages in device listeners.

## Key method hooks

The primary method hooks you can override are -

 - `setup()` : called whenever a brick / bricklet connection is made (field injected) and any time the code is updated.
 - `update()` : called on every update cycle _when a brick / bricklet is connected_.
 - `dispose()` : called when the component is about to be disconnected from the brick / bricklet. Should remove listeners and set callback rates back to 0.

## Additional functions / variables

### lcdString()

```java
String lcdString(String string, int length);
```

This function takes any String, truncates it to the required length and translates characters as required for display with the TinkerForge LCD bricklet.

### CALLBACK_PERIOD

```java
final int CALLBACK_PERIOD
```

This constant should be used whenever a callback period value needs to be set on a Brick or Bricklet

## Example

This is the code used to wrap the TinkerForge infra-red distance sensor within _Praxis LIVE_

```java
@TinkerForge BrickletDistanceIR device;
    
@P(1) @ReadOnly
int distance;
@Out(1) @ID("distance")
Output out;
  
Listener listener = new Listener();
    
@Override
public void setup() {
  device.addDistanceListener(listener);
  try {
    device.setDistanceCallbackPeriod(CALLBACK_PERIOD);
  } catch (TimeoutException | NotConnectedException ex) {
    log(WARNING, ex);
  }
}

@Override
public void dispose() {
  device.removeDistanceListener(listener);
  try {
    device.setDistanceCallbackPeriod(0);
  } catch (TimeoutException | NotConnectedException ex) {
    log(WARNING, ex);
  }
}
    
private class Listener implements BrickletDistanceIR.DistanceListener {

  @Override
  public void distance(int dist) {
    distance = dist;
      out.send(dist);
    }
       
}   
```
