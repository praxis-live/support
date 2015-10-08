# Audio components

## audio:custom

Base for audio components. No ports or controls by default. See coding guide.

## audio:gain

Control audio gain (volume). Multiple gain components can be controlled as one using the `link` port.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **level** : _Property (number 0..2)_ : gain level.
- **link** : _Link_ : Link gain components together (the level parameter will be synced).

## audio:input

Input from audio device. An input is optional in an audio graph. Supports 1-16 channels (default 2). Ports will be added dynamically.

- **out-1** : _AudioOut_
- **out-2** : _AudioOut_
- **channels** : _Property (1..16)_ : number of channels.

## audio:output

Output to audio device. An output is required in an audio graph. Supports 1-16 channels (default 2). Ports will be added dynamically.

- **in-1** : _AudioIn_
- **in-2** : _AudioIn_
- **channels** : _Property (1..16)_ : number of channels.

## audio:sampleplayer

A mono sample player. Supports looping, in/out points, variable speed.

Samples are loaded in the background. If using the `sample` port to load new samples, make use of the `ready` and `error` ports to track loading.

The player will not start automatically. To get it to start when the audio root is started, use a `core:start-trigger` attached to the play port.

- **out** : _AudioOut_
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


## audio:sine

Simple sine wave oscillator

- **out** : _AudioOut_
- **frequency** : _Property (number 110..1760)_

## audio:analysis:level

Calculate the level (volume) of the audio buffer using RMS.

The control cycle is always processed before the audio is calculated, therefore the output of this component will always be for the previous audio buffer.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **level** : _ControlOut_

## audio:container:input

Audio input for use inside a `core:container`

- **out** : _AudioOut_

## audio:container:output

Audio output for use inside a `core:container`

- **in** : _AudioIn_

## audio:delay:mono-delay

A simple mono delay (echo) component.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **time** : _Property (number 0..2)_ : delay time in seconds
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input
- **mix** : _Property (number 0..1)_

## audio:distortion:overdrive

A simple overdrive distortion effect.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **drive** : _Property (number 0..1)_ : distortion level
- **mix** : _Property (number 0..1)_

## audio:filter:comb

A comb filter.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **frequency** : _Property (number 20..20000)_ : frequency of filter
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input
- **mix** : _Property (number 0..1)_

## audio:filter:iir

An Infinite impulse response (IIR) filter with various types.

Types are LP6, LP12, HP12, BP12, NP12, LP24, HP24.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **frequency** : _Property (number)_ : frequency of filter
- **resonance** : _Property (number 0..30)_ : resonance of filter
- **mix** : _Property (number 0..1)_


## audio:mix:xfader

Crossfade between two mono signals. When `mix` is set to 0 or 1 processing of the unused channel will be switched off.

Currently limited to linear fading.

- **in-1** : _AudioIn_
- **in-2** : _AudioIn_
- **out** : _AudioOut_
- **mix** : _Property (number 0..1)_

## audio:modulation:chorus

A mono chorus component.

This is similar to `audio:modulation:lfo-delay` but with more limited range on parameters.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **depth** : _Property (number 0..40)_ : depth of chorus measured in milliseconds
- **rate** : _Property (number 0..15)_ : rate of LFO in Hz
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input
- **mix** : _Property (number 0..1)_

## audio:modulation:lfo-delay

A mono delay with delay time controlled by a low frequency oscillator.

This is similar to `audio:modulation:chorus` but with a wider range on parameters.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **time** : _Property (number 0..1)_ : average delay time in seconds
- **range** : _Property (number 0..1)_ : depth of LFO as proportion of delay time
- **rate** : _Property (number 0..40)_ : rate of LFO in Hz
- **feedback** : _Property (number 0..1)_ : amount of signal fed back into input
- **mix** : _Property (number 0..1)_

## audio:reverb:freeverb

A stereo reverb, port of the popular Freeverb.

- **in-1** : _AudioIn_
- **in-2** : _AudioIn_
- **out-1** : _AudioOut_
- **out-2** : _AudioOut_
- **room-size** : _Property (number 0..1)_
- **damp** : _Property (number 0..1)_ : amount of damping
- **width** : _Property (number 0..1)_ : stereo width
- **wet** : _Property (number 0..1)_ : level of reverberated signal
- **dry** : _Property (number 0..1)_ : level of original input signal (0.5 is original level)

## audio:sampling:looper

A mono phrase sampler. Supports in/out points, variable speed.

- **in** : _AudioIn_
- **out** : _AudioOut_
- **position** : _Property (number 0..1, transient)_ : normalized playback position
- **start** : _Property (number 0..1)_ : start point for playback / looping
- **end** : _Property (number 0..1)_ : end point for playback / looping
- **speed** : _Property (number -4..4)_ : playback speed (supports reverse play)
- **loop** : _Property (boolean)_ : loop playback
- **play** : _Action_ : start playback
- **stop** : _Action_ : stop playback
- **record** : _Action_ : start recording

## audio:sampling:player

A stereo version of `audio:sampleplayer`. Supports looping, in/out points, variable speed.

Samples are loaded in the background. If using the `sample` port to load new samples, make use of the `ready` and `error` ports to track loading.

The player will not start automatically. To get it to start when the audio root is started, use a `core:start-trigger` attached to the play port.

- **out** : _AudioOut_
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



