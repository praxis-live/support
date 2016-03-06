# Core components

Core components can be used with all files (audio, video, tinkerforge, etc.) in the graph editor.

## core:container

@TODO

## core:custom

Base for core (not audio, video, etc.) components. No ports or controls by default. See [coding guide](coding.md).

## core:property

Stores a property, and sends it from its output port whenever a new value is received. Also sends its value when the root container starts (eg. audio starts playing).

- **out** : _ControlOut_
- **value** : _Property_ : set / get value.

## core:start-trigger

Output a signal (empty value) when the root container starts.

- **out** : _ControlOut_

## core:tracker

Tracker sequencing component, supporting 8 channels and multiple patterns. The `trigger` port would normally be connected to a `core:timing:timer` or `audio:clock` component.

Use `Edit patterns` from the component popup menu to open the pattern table editor. Use `Save (CTRL-S)` to update the component.

- **out-N** : _ControlOut_ : output for N channel
- **patterns** : _Property_ : pattern data
- **pattern** : _Property (integer 0..)_ : current pattern
- **position** : _Property (integer 0.., transient)_ : current row position in pattern
- **trigger** : _Action_ : send current row from outs and increment position
- **reset** : _Action_ : reset position to 0

## core:variable

Stores a value and sends it whenever triggered. Unlike `core:property` it does not automatically send on each update.

- **out** : _ControlOut_
- **value** : _Property_ : set / get value.
- **trigger** : _Action_ : trigger output.

## core:array:iterator

Loops through an array of arguments, jumping to the next each time it is triggered. Can be set to jump a (random) number of elements ahead, determined by `min-skip` and `max-skip`. Can loop continuously forwards, or ping-pong back and forth through the array.

If the array is empty, an empty string will be sent instead.

- **out** : _ControlOut_
- **values** : _Property (array)_ : set / get values array.
- **index** : _Property (read-only)_ : current position.
- **trigger** : _Action_ : trigger output.
- **min-skip** : _Property (number 1..)_ : minimum count to move on trigger.
- **max-skip** : _Property (number 1..)_ : maximum count to move on trigger.
- **ping-pong** : _Property (boolean)_ : loop back and forth.
- **reset-on-change** : _Property (boolean)_ : reset index to 0 when `values` changes.
- **trigger** : _Action_ : trigger output and increment position.
- **reset** : _Action_ : reset index to 0.

## core:array:random

Stores an array (list) of arguments, and selects and sends one randomly when triggered.

If the array is empty, an empty string will be sent instead.

- **out** : _ControlOut_
- **values** : _Property (array)_ : set / get values array.
- **trigger** : _Action_ : trigger random output.

## core:container:input

## core:container:output

## core:container:property

## core:math:add

Add value to input.

- **in** : _ControlIn (number)_
- **out** : _ControlOut_
- **value** : _Property (number)_

## core:math:multiply

Multiply input by value.

- **in** : _ControlIn (number)_
- **out** : _ControlOut_
- **value** : _Property (number)_

## core:math:random

Sends a random number between minimum and minimum + range when triggered

If range is zero, the value of minimum will be sent when triggered.

- **out** : _ControlOut_
- **minimum** : _Property (number)_
- **range** : _Property (number 0..)_
- **trigger** : _Action_ : trigger output.

## core:math:scale

Takes a number between x1 and x2 and scales it between y1 and y2. The input value will be clamped between x1 and x2 before scaling. x1 may be higher than x2, and y1 may be higher than y2 - this allows inverse scaling.

- **in** : _ControlIn (number)_
- **out** : _ControlOut_
- **x1** : _Property (number)_
- **x2** : _Property (number)_
- **y1** : _Property (number)_
- **y2** : _Property (number)_

## core:math:threshold

Takes a number on its input port and sends it from its out-high port if greater than or equal to the threshold value, or from the out-low port if below the threshold value.

- **in** : _ControlIn (number)_
- **out-low** : _ControlOut_
- **out-high** : _ControlOut_
- **threshold** : _Property (number)_

## core:routing:gate

A gate for control signals. Stops signals getting through when inactive.

The pattern property offers rudimentary sequencing support. It accepts an array of numbers between 0 and 1. The pattern loops through with each input signal (when active). A value of 0 means the signal is discarded. A value of 1 means the signal gets through. Values in-between mean that the signal will sometimes get through - 0.5 means the signal will get through on average half of the time, 0.25 means quarter of the time, etc. An empty pattern is ignored and the value of active is used as is.

- **in** : _ControlIn_
- **out** : _ControlOut_ : values allowed through the gate.
- **discard** : _ControlOut_ : values not allowed through the gate.
- **active** : _Property (boolean)_
- **pattern** : _Property (array of number 0..1)_ : see description.
- **retrigger** : _Action_ : restart pattern sequence.

## core:routing:inhibitor

Stop more than one signal being sent through the component in a period of time.

- **in** : _ControlIn_
- **out** : _ControlOut_
- **time** : _Property (number 0..60)_ : minimum time between signals (in seconds).

## core:routing:join

Join two input signals. A signal to either input port will arm the component. A signal to the other input port will then pass through, and un-arm the component.

Repeated signals to the arming port will be ignored.

- **in-1** : _ControlIn_
- **in-2** : _ControlIn_
- **out** : _ControlOut_
- **reset** : _Action_ : un-arm component.

## core:routing:send

Send the input signal to a control address. Primarily used for sending to controls in different roots.

- **in** : _ControlIn_
- **address** : _Property (control address)_

## core:timing:animator

Animate to a value.

- **out** : _ControlOut (number)_
- **to** : _Property (number, transient)_ : value to animate to.
- **value** : _Property (number) : set / get current value; setting value will stop animation.
- **time** : _Property (number 0..60)_ : animation time in seconds.

## core:timing:delay

Delay an input signal for a period of time.

A delay of 0 will delay the signal until the next control period (video frame, audio buffer, etc.) is processed.

- **in** : _ControlIn_
- **out** : _ControlOut_
- **time** : _Property (number 0.60)_ : delay time in seconds.

## core:timing:timer

A simple timer. Sends a signal (empty value) every period.

- **out** : _ControlOut_
- **period** : _Property (number 0..60)_ : timer period in seconds.
