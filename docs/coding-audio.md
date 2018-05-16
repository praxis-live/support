# Coding audio components

`audio:custom` is the base type for audio components. You have access to the same
functions as `core:custom` as well as a range of audio processing blocks and related
functionality.

## Unit Generators (UGens)

Like many other environments, audio processing is achieved by interconnecting a
series of audio processing blocks, otherwise known as
[unit generators](https://en.wikipedia.org/wiki/Unit_generator) (UGens). Inside audio
components these UGens are subclasses of `Pipe`, from the
[JAudioLibs Pipes library](https://github.com/jaudiolibs/pipes). This is the same
routing library used throughout the audio pipeline in _PraxisLIVE_. There is no
difference in performance between connections made in code or graphically - group
things in the way that makes most sense to you!

Unit generators are created by placing the `@UGen` annotation on fields of the
relevant types. Multiple fields of the same type may be annotated together -

```java
@UGen Osc osc1, osc2;
@UGen IIRFilter filter;
```

Using annotations in this way, UGens of the same name and type are copied across as
the code updates, ensuring glitch free playback while updating code.

See a full list of available UGens further down this page.

## Per-sample functions

It is also possible to write per-sample processing functions, with support for lambdas.
See the documentation for `fn()` and `modFn()` under Audio routing.

## Input / Output

Audio input and output ports are defined by use of the `@In` and `@Out` annotations on
fields of type `AudioIn` and `AudioOut`. These types are also subclasses of `Pipe`.

eg. -

```java
@In(1) AudioIn in1;
@In(2) AudioIn in2;
@Out(1) AudioOut out;
```

You also have access to the same control input and output functionality as [core components](coding-core.md).

## Key method hooks

The key method hooks you can override are -

 - `init()` : called each time the pipeline is started and any time the code is updated.
Because this method is called on code updates, it needs to be suitable for real-time usage.
**NB. All UGens will be reset and disconnected prior to this method being called**
 - `update()` : called every update cycle before the next audio buffer is processed.
If your component reacts solely to input and this method would otherwise be empty,
remove it for efficiency.

## Audio routing

Audio routing should usually be done inside the `init()` method, although dynamic
changes to the audio processing graph are possible. Note that some UGens, such as `Gain`,
provide the ability to dynamically switch off parts of the audio graph for efficiency
without a need for dynamic connections.

Prior to `init()` being called, all UGens are disconnected from each other and all
parameters are reset - this ensures that the routing always respects the code you write.

Be careful not to try and link UGens more times than they allow - most UGens only
support one output, and zero or one input.

### link()

```java
Pipe link(Pipe ... ugens)
```

Link the UGens together in a chain. For convenience the last UGen provided is returned (see example).

### add()

```java
Add add(Pipe ... ugens)
```

Add the outputs of the provided UGens together. This method creates an `Add` UGen,
and connects the provided UGens as sources. The returned UGen supports unlimited
inputs and sources may be added later.

### fn()

```java
OpGen fn(DoubleUnaryOperator function)
```

Shortcut for creating an OpGen with provided function inside a pipeline. The
returned UGen supports a single input and output (input is optional).

eg. assuming a phase input from 0..1, a simple sin oscillator function would be

```java
fn(d -> sin(d * TWO_PI))
```

### mod()

```java
Mod mod(Pipe ... ugens)
```

Multiply the outputs of UGens together. This method creates a `Mod` UGen,
and connects the provided UGens as sources. The returned UGen supports unlimited
inputs and sources may be added later.

### modFn()

```java
Mod modFn(DoubleBinaryOperator function)
Mod modFn(Pipe ugen1, DoubleBinaryOperator function)
```

Combine the outputs of UGens together using the provided function. This method
creates a `Mod` UGen, and connects the optional UGen as a source. The returned UGen
supports unlimited inputs and sources may be added later.

eg. the equivalent of the standard mod() function would be -

```java
modFn(in1, (in1, in2) -> in1 * in2);
```

### tee()

```java
Tee tee();
```

Construct a `Tee` UGen. The returned UGen supports an unlimited number of outputs.
It is useful for splitting a signal into multiple chains, or creating feedback loops.

### Example usage

The routing methods support nested and un-nested usage. Use whichever feels most comfortable.

```java
@In(1) AudioIn in1;
@In(2) AudioIn in2;
@In(3) AudioIn modulator;
@Out(1) AudioOut out;

@UGen IIRFilter filter1, filter2;

public void setup() {
  link(
    mod(
      add(
        link(in1, filter1);
        link(in2, filter2);
      ),
      modulator
    ),
    out
  );
  // OR
  link(in1, filter1);
  link(in2, filter2);
  Add a = add(filter1, filter2);
  Mod m = mod(a, modulator);
  link(m, out);
}
```

## Loading samples

Use the [`@P`](annotations.md#p) annotation on a field of type `AudioTable` to
support loading of samples from a file. Loading is done asynchronously.

The field value may be `null` - UGens will handle this, but be careful in your
own code. Use the [`@OnChange`](annotations-additional.md#onchange) annotation to
update UGens with a new table -

```java
@UGen Player player;
@P(1) @OnChange("updateSample") AudioTable sample;

public void setup() {
  // rest of setup
  updateSample();
}

void updateSample() {
  player.table(sample);
}
```

## Additional variables and functions

In addition to the core Processing API functions, a range of specific audio-related
functions and variables are available to your code.

### Samplerate and block size

```java
double sampleRate;
int blockSize;
```

These variables provide access to the sample rate and block size (number of samples
processed in one update). They should be treated as read-only.

### Notes and frequency

```java
double noteToFrequency(String note)
int noteToMidi(String note)
double midiToFrequency(int midi)
```

These methods convert from a MIDI note number or a text representation of a note (a4, f#3, etc.)
into a frequency in Hz.

## Available UGens

All built-in UGens support a fluid programming style. Property setters take a value and return
the UGen. Property getters take no value and return the value of the property.

eg. -

```java
IIRFilter filter;

filter.frequency(880).resonance(10).type(IIRFilter.LP12);
double freq = filter.frequency();

```

### Chorus

A mono chorus effect. One input, one output.

```java
@UGen Chorus chorus;

chorus.depth(double ms); // depth of chorus in ms, default 0
chorus.rate(double hz); // rate of chorus in hz, default 0
chorus.feedback(double f); // feedback (0, none - 1, 100%), default 0
```

### Comb Filter

A mono comb filter effect. One input, one output.

```java
@UGen CombFilter comb;

comb.frequency(double hz); // frequency of filter in Hz, default 20Hz
comb.feedback(double f); // feedback (0, none - 1, 100%), default 0
```

### Delay

Mono delay effect, with maximum delay time of 2 seconds. One input, one output.

```java
@UGen Delay delay;

delay.time(double s); // delay time in seconds (0..2), default 0
delay.feedback(double f); // feedback (0, none - 1, 100%), default 0
delay.level(double l); // level of delay (0, none - 1, 100%), default 1
delay.passthrough(boolean p); // whether to add input signal, default false
```

### Freeverb

A mono or stereo reverb effect. One or two inputs, one or two outputs.

```java
@Ugen Freeverb reverb;

reverb.damp(double d); // amount of damping (0..1), default 0.5
reverb.dry(double d); // level of dry signal (0..1), default 0.5
reverb.roomSize(double s); // room size, (0..1), default 0.5
reverb.wet(double w); // level of wet signal (0..1), default 0
reverb.width(double w); // width of stereo signal (0..1), default 0.5
```

### Gain

Mono gain. One input, one output.

The level value is interpolated over buffer so suitable for envelopes. The level
value is used as a linear multiplier. **A level of 0 will result in input being
switched off (no processing required)**.

```java
@UGen Gain gain;

gain.level(double g); // value by which input is multiplied.
```

### IIR Filter

A mono IIR filter with various types (LP6, LP12, LP24, HP12, HP24, BP12, NP12).
Types are available as static fields - eg. use `IIRFilter.LP12`

```java
@UGen IIRFilter filter;

filter.frequency(double hz); // frequency in Hz, default 20000
filter.resonance(double db); // resonance in dB (0..30), default 0
filter.type(IIRFilterOp.Type type); // filter type from list above, default IIRFilter.LP6
```

### LFO

A low frequency oscillator with various waveforms (Sine, Square and Saw),
and unipolar (0..1) or bipolar (-1..1) output. Unlike the Osc waveforms,
these are not bandlimited. One output.

```java
@UGen LFO lfo;

lfo.bipolar(boolean bp); // bipolar output, default false (unipolar)
lfo.frequency(double hz); // frequency in Hz, default 1
lfo.gain(double g); // linear gain of waveform (0..1), default 1
lfo.waveform(Waveform w); // waveform, one of SINE, SQUARE or SAW, default SINE
```

### LFO Delay

A mono delay effect with delay time controlled by an LFO - similar to the chorus effect.
One input, one output.

```java
@UGen LFODelay delay;

delay.time(double s); // delay time in seconds (0..1), default 0
delay.feedback(double f); // feedback (0, none - 1, 100%), default 0
delay.range(double r); // range of lfo sweep (0..1), default 0
delay.rate(double hz); // rate of lfo in Hz, default 0
```

### Looper

A multi-channel sample looper (similar to player but with inputs and record
functionality).

See code of the included `audio:looper` component for an example of how to manage
AudioTable for use with Looper.

```java
@UGen Looper looper;

looper.table(Table t); // sample table to play and record to, or null, default null
looper.in(double i); // loop in / start point (0..1), default 0
looper.out(double o); // loop out / stop point (0..1), default 1
looper.position(double p); // playback position (0..1), no default - not reset
looper.speed(double s); // playback speed, default 1
looper.looping(boolean lp); // loop playback, default false
looper.recording(boolean rec); // record input, default false

looper.play(); // start playing
looper.stop(); // stop playing
```

### OpGen (function holder)

A mono UGen that allows a per-sample function to be applied. One (optional) input, one output.

The preferred usage of OpGen is via `fn()` as described in the routing section.

```java
@UGen OpGen op;

op.function(d -> d); // default is passthrough Function as shown.
```

### Oscillator (Osc)

An oscillator with various waveforms (Sine, Square and Saw). The waveforms are bandlimited
where necessary. One output.

```java
@UGen Osc osc;

osc.frequency(double hz); // frequency in Hz, default 440
osc.gain(double g); // linear gain of waveform (0..1), default 1
osc.waveform(Waveform w); // waveform, one of SINE, SQUARE or SAW, default SINE
```

### Overdrive

A simple mono overdrive effect. One input, one output.

```java
@UGen Overdrive od;

od.drive(double amt); // drive (0..1), default 0
```

### Player

A multi-channel sample table player. This UGen currently supports up to 16 outputs.

The output channel of the table is modulo the output channel of the UGen.
eg. if a Player has three outputs and the table (sample) has two channels,
then outputs 1 & 3 will both receive sample channel 1.

```java
@UGen Player player;

player.table(Table t); // sample table to play or null, default null
player.in(double i); // loop in / start point (0..1), default 0
player.out(double o); // loop out / stop point (0..1), default 1
player.position(double p); // playback position (0..1), no default - not reset
player.speed(double s); // playback speed, default 1
player.looping(boolean lp); // loop playback, default false

player.play(); // start playing
player.stop(); // stop playing
```

### Phasor

Similar to a sawtooth LFO, but with controllable range (including reverse).

```java
@UGen Phasor phasor;

phasor.frequency(double hz); // frequency in Hz, default 1
phasor.minimum(double min); // minimum value of range, default 0
phasor.maximum(double max); // maximum value of range, default 1
phasor.phase(double ph); // phase position between 0 and 1
```