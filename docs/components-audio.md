# Audio components

## audio:clock

A timer for audio patches synchronized to the sample rate clock. Sends a signal (empty value) every period.

For consistency, the pulse is quantized to a set number of audio buffers, so may differ slightly from the requested value.

- **out** : _ControlOut_
- **bpm** : _Property (number 10..300)_ : beats per minute.
- **subdivision** : _Property (integer 1..16)_ : number of pulses per beat (eg. default 4 is equivalent to semiquaver).
- **actual-bpm** : _Property (number, read-only)_ : the actual bpm taking into account quantization.
- **period** : _Property (number, read-only)_ : the actual period in seconds.
- **buffer-count** : _Property (integer, read-only)_ : the number of audio buffers per pulse

## audio:custom

Base for audio components. No ports or controls by default. See [coding guide](coding.md).

## audio:gain

Control audio gain (volume).

- **in** : _AudioIn_
- **out** : _AudioOut_
- **level** : _Property (number 0..2)_ : gain level.

## audio:input

Input from audio device. An input is optional in an audio graph. Supports 1-16 channels (default 2).
Ports will be added dynamically.

- **out-1** : _AudioOut_
- **out-2** : _AudioOut_
- **channels** : _Property (1..16)_ : number of channels.

## audio:looper

A stereo sample looper. Supports recording, looping, in/out points, variable speed.

The looper will not start automatically. To get it to start when the audio root is
started, use a `core:start-trigger` attached to the play port.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **recording** : _Property (boolean)_ : record input (must be playing)
- **play** : _Action_ : start playback
- **stop** : _Action_ : stop playback
- **position** : _Property (number 0..1, transient)_ : normalized playback position
- **start** : _Property (number 0..1)_ : start point for playback / looping
- **end** : _Property (number 0..1)_ : end point for playback / looping
- **speed** : _Property (number -4..4)_ : playback speed (supports reverse play)
- **loop** : _Property (boolean)_ : loop playback
- **playing** : _Property (boolean, transient)_ : playing
- **loop-size** : _Property (number 0..5)_ : loop size in seconds

## audio:osc

Simple oscillator

- **out** : _AudioOut_
- **waveform** : _Property (Sine, Square, Saw)_ 
- **frequency** : _Property (number 20..3600)_
- **level** : _Property (number 0..1)_

## audio:output

Output to audio device. An output is required in an audio graph. Supports 1-16 channels (default 2).
Ports will be added dynamically.

- **in-1** : _AudioIn_
- **in-2** : _AudioIn_
- **channels** : _Property (1..16)_ : number of channels.

## audio:player

A stereo sample player. Supports looping, in/out points, variable speed.

Samples are loaded in the background. If using the `sample` port to load new
samples, make use of the `ready` and `error` ports to track loading.

The player will not start automatically. To get it to start when the audio root is
started, use a `core:start-trigger` attached to the play port.

- **out-1 / out-2** : _AudioOut_
- **sample** : _Property (empty or resource)_ : audio file
- **position** : _Property (number 0..1, transient)_ : normalized playback position
- **start** : _Property (number 0..1)_ : start point for playback / looping
- **end** : _Property (number 0..1)_ : end point for playback / looping
- **speed** : _Property (number -4..4)_ : playback speed (supports reverse play)
- **loop** : _Property (boolean)_ : loop playback
- **play** : _Action_ : start playback
- **stop** : _Action_ : stop playback
- **ready** : _ControlOut_ : signal for new sample loaded
- **error** : _ControlOut_ : signal for error during new sample loading

## audio:analysis:level

Calculate the level (volume) of the audio buffer using RMS.

The control cycle is always processed before the audio is calculated, therefore the
output of this component will always be for the previous audio buffer.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **level** : _ControlOut_

## audio:fx:chorus

A chorus component.

This is similar to `audio:fx:lfo-delay` but with more limited range on parameters.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **depth** : _Property (number 0..40)_ : depth of chorus measured in milliseconds
- **rate** : _Property (number 0..15)_ : rate of LFO in Hz
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input

## audio:fx:comb-filter

A comb filter.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **frequency** : _Property (number 20..20000)_ : frequency of filter
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input

## audio:fx:delay

A simple delay (echo) component.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **time** : _Property (number 0..2)_ : delay time in seconds
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input

## audio:fx:filter

An Infinite impulse response (IIR) filter with various types.

Types are LP6, LP12, HP12, BP12, NP12, LP24, HP24.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **type** : _Property (in Types)_
- **frequency** : _Property (number)_ : frequency of filter
- **resonance** : _Property (number 0..30)_ : resonance of filter

## audio:fx:lfo-delay

A delay with delay time controlled by a low frequency oscillator.

This is similar to `audio:modulation:chorus` but with a wider range on parameters.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **time** : _Property (number 0..1)_ : average delay time in seconds
- **range** : _Property (number 0..1)_ : depth of LFO as proportion of delay time
- **rate** : _Property (number 0..40)_ : rate of LFO in Hz
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input

## audio:fx:overdrive

A simple overdrive distortion effect.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **drive** : _Property (number 0..1)_ : distortion level

## audio:fx:reverb

A stereo reverb, port of the popular Freeverb.

- **in-1 / in-2** : _AudioIn_
- **out-1 / out-2** : _AudioOut_
- **room-size** : _Property (number 0..1)_
- **damp** : _Property (number 0..1)_ : amount of damping
- **width** : _Property (number 0..1)_ : stereo width
- **wet** : _Property (number 0..1)_ : level of reverberated signal
- **dry** : _Property (number 0..1)_ : level of original input signal (0.5 is original level)

## audio:mix:xfader

Crossfade between two mono signals. When `mix` is set to 0 or 1 processing of the unused channel will be switched off.

Currently limited to linear fading.

- **in-1** : _AudioIn_
- **in-2** : _AudioIn_
- **out** : _AudioOut_
- **mix** : _Property (number 0..1)_

