# Coding video components

`video:custom` is the base type for components that work with video, and can be used in video graphs using software and OpenGL rendering. You have access to a limited subset of the Processing API for graphical operations, as well as most of the Processing API not related to graphics. This does not include functions not suitable for real-time operation (would normally only be advised in `setup()` within Processing).

If you need full access to the Processing graphics API / OpenGL coding, see [Coding Video GL components](coding-video-gl.md) instead.

## Key method hooks

The primary method hooks you can override are -

 - `setup()` : called each time the pipeline is started and any time the code is updated. Because this method is called on code updates, unlike in Processing it needs to be suitable for real-time usage. Unlike other components, setup is called in the render cycle so like in Processing you can draw to the output.
 - `draw()` : called every render cycle. Draw to the output.
 
_Be aware that `setup()` and `draw()` are called in the rendering part of the processing cycle._ You should not send control signals during these methods. This also means that `setup()` is _not_ guaranteed to be called before fields / methods on your component are accessed by the update part of the processing cycle.

## Video input / output

All `video:custom` components have an automatically generated video output port (`out`) on which drawing operations are performed. It is not currently possible to rename this port or add additional video outputs.

Video input ports can be created by annotating a `PImage` field with the [`@In`](annotations.md#in) annotation.

The `PImage` type in _PraxisLIVE_ is read-only and does not support any of the methods from Processing. It does have fields for width and height.

## Loading images

It is possible to load an image from a file by annotating a `PImage` field with the [`@P`](annotations.md#p) annotation. Note that in this case the field may be `null`.

All image loading is done aynchronously - the new image will be available sometime after the current video frame. To know when the image is loaded, or be informed of errors, use the [`@OnChange`](annotations-additional.md#onchange) and [`@OnError`](annotations-additional.md#onerror) annotations. _Note that methods called by these annotations are called in the update part of the processing cycle - you cannot draw in them, but you can send output_.

## Offscreen graphics

It is possible to create an offscreen rendering surface, for example to grab a still image or cache an expensive rendering operation. Use the [`@OffScreen`](annotations.md#offscreen) annotation on a `PGraphics` field. By default the surface will have the same dimensions and format (RGB / ARGB) as the output, but this can be overridden using the annotation parameters.

## Zero-copy images

It is important to note that _PraxisLIVE_ has an important extension to the Processing API to support efficient passing of video frames through the pipeline. If you want to copy an image input to the output or an offscreen graphics you should not use -

```java
image(in, 0, 0);
// OR
offscreen.image(in, 0, 0);
```

but use -

```java
copy(in);
release(in);
// OR
offscreen.copy(in);
release(in);
```

This passes through a reference to the underlying image data rather than rendering it. _PraxisLIVE_ tracks use of the image data, and whether it is being read or written to, only copying the data when required. In the given example you should not use the `in` image again after releasing it until the next frame - it will contain no / random data.

You can also use `copy()` on an image loaded from a file, but in this case do not release it unless you are really finished with the image!

## Blend modes

_PraxisLIVE_ works differently to the standard Processing API with respect to blending modes. Blend mode definitions are based on common usage, which may not always match Processing's definitions. _PraxisLIVE_ also works throughout with pre-multiplied alpha blending to ensure blending behaves as expected throughout the pipeline (Processing is broken in this respect!)

As well as specifiying a blend mode for rendering you can also specify an opacity.

eg. 

```java
blendMode(DIFFERENCE, 0.7); // Difference blending with 70% opacity
rect(0, 0, width / 2, height);

blendMode(BLEND); // Normal blending with 100% opacity - equivalent to blendMode(BLEND, 1);
image(sprite, 10, 10);
```

Currently available blend modes are

 - `BLEND` : Standard (default) blend, equivalent to SrcOver
 - `ADD` : Additive blending
 - `SUBTRACT` : Subtractive blending
 - `DIFFERENCE` : Difference blending (may behave differently to Processing)
 - `MULTIPLY` : Multiply blending (behaves correctly with alpha channel - no effect as approaches transparency)
 - `SCREEN` : Screen blending
 - `BITXOR` : Binary XOR
 - `MASK` : Multiply blending that also multiplies alpha channel - useful for applying a mask. Behaves the same as MULTIPLY with opaque images.

